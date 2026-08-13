# EVOLUMA Studio — distribuição do beta

Este repositório **não contém o código-fonte** do EVOLUMA Studio. Ele existe
para duas coisas apenas:

1. **Hospedar as duas páginas** (GitHub Pages, servidas da pasta `docs/`):
   - `index.html` — formulário público de inscrição no beta;
   - `admin.html` — painel de acompanhamento e aprovação (login do dono).
2. **Distribuir os instaladores** nas *Releases*, com o manifesto que o
   aplicativo lê para se atualizar sozinho.

## Por que as páginas não moram no Supabase

O domínio `*.supabase.co` devolve `text/plain` + `nosniff` para qualquer HTML —
tanto em Edge Functions quanto em Storage. É uma trava anti-phishing da
plataforma, verificada nas duas pontas. O que fica no Supabase é só o endpoint
JSON que recebe a inscrição (`/functions/v1/inscricao`) e o banco.

## Publicar as páginas

Em **Settings → Pages**, escolher `Deploy from a branch`, branch `main`, pasta
`/docs`. Os endereços passam a ser:

- formulário: `https://eudavidthomaz.github.io/evoluma/`
- painel: `https://eudavidthomaz.github.io/evoluma/admin.html`

O painel exige login de administrador; sem ele o banco não devolve linha
alguma, porque quem barra é a política RLS, não a página.

## Publicar uma versão

Cada release leva quatro arquivos:

| Arquivo | Para quê |
|---|---|
| `EVOLUMA Studio_<versão>_aarch64.dmg` | primeira instalação |
| `EVOLUMA Studio.app.tar.gz` | o pacote que o atualizador baixa |
| `EVOLUMA Studio.app.tar.gz.sig` | assinatura do pacote |
| `latest.json` | manifesto que o aplicativo consulta |

O aplicativo checa `releases/latest/download/latest.json` no arranque e só
instala um pacote cuja assinatura casa com a chave pública embutida no binário.
A chave privada **não** vive aqui — fica em `.tauri-keys/` na máquina de quem
compila, fora do controle de versão.

## Privacidade

Nenhum dado de fotógrafo passa por este repositório. As inscrições vão direto
do navegador para o Supabase; a lista de inscritos é fechada por RLS e só o
administrador lê. Os números do grupo do WhatsApp, quando usados para
cruzamento, ficam apenas no navegador de quem administra.
