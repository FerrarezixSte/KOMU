# KOMU

Rede social estilo Twitter feita com HTML, CSS e JavaScript puro. O site fica no **GitHub Pages** e os dados (contas, posts, fotos, seguidores) ficam no **Supabase**. Os dois são gratuitos.

## Arquivos

| Arquivo | Para que serve |
|---|---|
| `index.html` | O site inteiro (a logo já vem embutida). Já está ligado ao seu projeto Supabase. |
| `favicon.png` | Ícone da aba do navegador |
| `supabase.sql` | Cria ou atualiza as tabelas e as regras de segurança do banco |
| `logo.png` / `icon.png` | Logo e punho com fundo transparente, para usar onde quiser |

## Funcionalidades

- Cadastro e login com e-mail e senha
- **Verificação em duas etapas** com app autenticador (Google Authenticator, Microsoft Authenticator, Authy…)
- **Português e inglês**: o idioma segue o navegador e pode ser trocado no menu ou nas Configurações
- **Perfil privado**: só seguidores aprovados veem os posts, e novos seguidores precisam pedir para seguir
- **Foto e capa de perfil**: as imagens são recortadas e reduzidas automaticamente
- **Seletor de emojis**, com 🇧🇷 ⭐ 🐂 😡 🦑 em destaque e mais 56 emojis
- Feed com as abas "Para você" e "Seguindo", posts de até 280 caracteres, respostas, curtidas, reposts, itens salvos e busca
- Assuntos do momento e aviso de posts novos em tempo real
- Tema escuro (padrão) e claro, nas cores preto, vermelho e branco
- Layout responsivo para celular

---

## Atualizar um site que já está no ar

1. **Banco:** abra **Supabase → SQL Editor → New query**, cole todo o `supabase.sql` e clique em **Run**. Esse script **não apaga** posts nem contas. Ele só cria o que falta e atualiza as regras. Pode rodar quantas vezes precisar.
2. **Site:** no GitHub, vá em **Add file → Upload files**, envie o `index.html` novo e clique em **Commit changes**. Em cerca de 1 minuto o site atualiza.

## Instalar do zero

1. Crie um projeto em [supabase.com](https://supabase.com) e rode o `supabase.sql` no **SQL Editor**.
2. Em **Project Settings → API Keys**, copie a URL e a chave **publishable**, e cole no começo do script do `index.html` (`SUPABASE_URL` e `SUPABASE_KEY`). Nunca use a chave `secret` / `service_role`.
3. Crie um repositório público no GitHub, envie `index.html` e `favicon.png` e ative **Settings → Pages → Deploy from a branch → main / (root)**.
4. No Supabase, em **Authentication → URL Configuration**, coloque o endereço do site em **Site URL** e em **Redirect URLs**.

## Como usar as novidades

- **Configurações:** clique na sua foto (embaixo, à esquerda; no celular, no topo) ou em **Configurações** no menu.
- **Foto e capa:** toque no ícone de câmera e depois em **Salvar**.
- **Perfil privado:** ative a chave em **Privacidade**. Os pedidos para seguir aparecem logo abaixo, com os botões Aceitar e Recusar. Um ponto vermelho na sua foto avisa quando há pedidos novos.
- **Verificação em duas etapas:** em **Segurança**, clique em **Ativar**, escaneie o QR code com o app autenticador e digite o código de 6 dígitos. A partir daí, o login pede o código do app depois da senha.

> **Se perder o celular com o app autenticador:** apague o fator de verificação da conta no painel do Supabase, em **Authentication → Users → (usuário) → MFA factors**.

## Personalizar

- **Cores:** altere `--accent` no começo do CSS. O vermelho da marca é `#a20e0c`.
- **Tema padrão:** troque `data-theme="dark"` por `data-theme="light"` na tag `<html>`.
- **Emojis:** edite as listas `EMOJI_TOP` (destaques) e `EMOJIS` no JavaScript.
- **Textos:** todos os textos ficam no objeto `I18N`, com `pt` e `en`.

## Limites do plano gratuito

O plano grátis do Supabase oferece 500 MB de banco, 1 GB de arquivos (fotos e capas) e 50 mil usuários ativos por mês. Projetos sem nenhum acesso por 7 dias são pausados, mas dá para reativar com um clique no painel.
