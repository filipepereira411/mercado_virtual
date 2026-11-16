[README.md](https://github.com/user-attachments/files/34907130/README.md)
# mercado_virtual# Mercado Virtual – Salmão e Pneus

Mercado virtual num **único ficheiro** (`mercado.html`), sem dependências e sem servidor. Tem duas partes separadas:

| Parte | O que é | Ligação |
|---|---|---|
| 🌐 **Site** | Loja de salmão e pneus, contas de cliente, carrinho e administração | `#mercado`, `#salmao`, `#pneus`, `#admin` |
| 📱 **App** | Check-in mobile para levantamento de encomendas | `#app-checkin` |

As duas partes partilham os mesmos dados (contas, produtos e encomendas).

## Como usar

1. Abra o `mercado.html` num navegador.
2. Para testar com tudo a funcionar de forma fiável, sirva a pasta com um servidor local:

   ```bash
   python3 -m http.server 8000
   ```

   e abra `http://localhost:8000/mercado.html`.

> Alguns navegadores (por exemplo o Firefox) tratam cada ficheiro aberto diretamente do disco como um site separado. Num servidor, isso não acontece.

## Rotas

| Ligação | Ecrã |
|---|---|
| `#mercado` | Loja completa (salmão + pneus) |
| `#salmao` | Só salmão |
| `#pneus` | Só pneus |
| `#admin` | Administração |
| `#app-checkin` | App: check-in |
| `#app-encomendas` | App: as minhas encomendas |
| `#app-conta` | App: login, registo, recuperação de palavra-passe |
| `#app-balcao` | App: balcão (funcionários) |

## Funcionalidades

### Site
- Catálogo de salmão e pneus com carrinho e total em euros.
- **Contas de cliente:** registo, login e "esqueci a palavra-passe" (código de 6 dígitos, válido 10 minutos).
- Para finalizar uma encomenda é preciso ter sessão iniciada. Cada encomenda recebe um **código de levantamento** de 4 dígitos.
- **Administração:**
  - **Produtos:** criar, editar, apagar e esconder.
  - **Encomendas:** ver tudo e mudar o estado (`Pendente`, `Paga`, `Check-in`, `Enviada`, `Entregue`, `Cancelada`). Mostra a hora do check-in.
  - **Clientes:** listar, repor a palavra-passe e apagar.
  - **Conta admin:** alterar utilizador e palavra-passe.

### App (check-in mobile)
- **📍 Check-in:** o cliente toca em "Cheguei" e a app mostra o código de levantamento em grande.
- **📦 Encomendas:** lista as encomendas do cliente e o estado de cada uma.
- **👤 Conta:** login, registo e recuperação de palavra-passe (as mesmas contas do site).
- **🧾 Balcão:** acesso de funcionário com as credenciais de administrador. Lista os clientes que fizeram check-in, permite procurar por código e marcar como `Entregue`.

## Fluxo típico

1. O cliente regista-se e faz a encomenda no site → recebe o código.
2. Ao chegar, abre a app e faz check-in.
3. O funcionário vê o cliente no **Balcão**, confere o código e entrega.
4. No `#admin`, a encomenda aparece como `Entregue`, com a hora do check-in.

## Credenciais por defeito

| Utilizador | Palavra-passe |
|---|---|
| `admin` | `admin` |

⚠ **Altere-as** em `#admin` → **Conta admin** antes de qualquer uso real. Enquanto estiverem por defeito, aparece um aviso a vermelho.

## Dados guardados

Tudo fica no `localStorage` do navegador (chaves com o prefixo `mk_`):

| Chave | Conteúdo |
|---|---|
| `mk_prods` | Produtos |
| `mk_users` | Clientes (palavras-passe com hash SHA-256) |
| `mk_orders` | Encomendas |
| `mk_sess` | Sessão do cliente |
| `mk_admin` | Credenciais do administrador |
| `mk_reset` | Código de recuperação pendente |

As sessões de administrador e de balcão usam `sessionStorage` (`mk_adm`, `mk_stf`).

**Repor tudo de fábrica:** nas ferramentas do navegador, apague as chaves `mk_*` do `localStorage` e recarregue a página.

## Limitações

- **Sem servidor:** cada pessoa só vê os seus próprios dados, no seu navegador. O balcão só vê check-ins feitos no mesmo aparelho e navegador.
- **Sem segurança real:** quem tiver conhecimentos técnicos consegue ver ou alterar os dados nas ferramentas do navegador. Serve para demonstração e testes, não para produção.
- **Recuperação de palavra-passe:** o código aparece no ecrã (simulação de email), porque não há envio de emails.
- **Sem pagamentos:** "Finalizar encomenda" só regista a encomenda.
- **App:** é uma página web otimizada para telemóvel. Pode ser aberta como app com "Adicionar ao ecrã principal", mas não é uma app nativa nem tem notificações.

## Próximos passos (para uma loja real)

- Servidor com base de dados (por exemplo Node + SQLite, ou Vercel) para partilhar dados entre aparelhos.
- Autenticação a sério (hash com sal, sessões, HTTPS) e envio de emails.
- Pagamentos (MB WAY, cartão, Multibanco).
- App instalável (PWA) com notificações de "encomenda pronta".
- Fotografias, stock e preços por quantidade.

## Estrutura

```
mercado.html   # site + app, tudo num só ficheiro
README.md      # este ficheiro
```
