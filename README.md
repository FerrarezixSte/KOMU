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

- **Conta:** cadastro com confirmação por e-mail (tela "Confirme seu e-mail" + botão de reenviar), senha forte obrigatória, alterar senha e verificação em duas etapas (app autenticador)
- **Meu perfil no topo do menu:** clicar abre o seu perfil; a engrenagem ao lado abre as Configurações (perfil, foto/capa, privacidade, segurança, **tema e idioma**)
- **Posts:** texto, até 4 fotos, 1 vídeo (até 25 MB), GIF ou figurinha; colar print direto no campo também funciona
- **Biblioteca da comunidade:** GIFs e figurinhas que as próprias pessoas enviam, com busca por palavra; 3 figurinhas oficiais da KOMU já vêm prontas
- **Página do post:** clicar num post abre ele com todas as respostas; dá para abrir o perfil de quem comentou
- **@ marcações:** ao digitar @ aparecem sugestões de pessoas
- **Notificações em tempo real:** marcações, respostas, novos seguidores e pedidos para seguir
- **Mensagens privadas** em tempo real (perfil privado só recebe de quem segue ou é seguido)
- **Explorar:** busca no centro da página, com pessoas e posts
- Perfil privado, denúncias, curtidas, reposts, salvos, link de compartilhamento do post
- Português e inglês · tema escuro e claro · layout para celular

---

## Atualizar um site que já está no ar

1. **Banco:** abra **Supabase → SQL Editor → New query**, apague o que estiver no editor, cole todo o `supabase.sql` e clique em **Run**. Esse script **não apaga** posts nem contas. Ele só cria o que falta e atualiza as regras. Pode rodar quantas vezes precisar.
   - No fim aparece uma tabela **Checklist**: todos os itens precisam estar com ✅.
   - Se aparecer um aviso sobre *destructive operation* ou *RLS*, clique em **Run this query**.
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

## E-mail de confirmação do cadastro

O envio de e-mails que vem de graça no Supabase **só entrega para os membros da equipe do projeto** e tem um limite de poucos e-mails por hora. Por isso, quem se cadastra não recebe o link. Há duas saídas:

**Opção A — rápida: dispensar a confirmação**
1. Supabase → **Authentication → Sign In / Providers → Email** → desligue **Confirm email** → **Save**.
2. Quem já se cadastrou e ficou sem confirmar: rode isto no SQL Editor:
   ```sql
   update auth.users set email_confirmed_at = now() where email_confirmed_at is null;
   ```

**Opção B — mandar e-mails de verdade (SMTP próprio)**
1. Crie uma conta num serviço de e-mail com plano gratuito, como o [Brevo](https://www.brevo.com) (300 e-mails/dia) ou o [Resend](https://resend.com).
2. Pegue os dados de SMTP do serviço (servidor, porta, usuário e senha).
3. Supabase → **Authentication → Emails → SMTP Settings** → ative **Enable custom SMTP** e cole os dados.
4. Em **Authentication → Rate Limits**, aumente o limite de e-mails por hora.

## Senha forte também no servidor (recomendado)

O site já exige senha forte, mas vale ligar a mesma regra no Supabase, para ninguém burlar pelo código:
**Authentication → Sign In / Providers → Email** → *Minimum password length* = **8** e *Password requirements* = **letras minúsculas, maiúsculas, números e símbolos** → **Save**.

## Verificação em duas etapas: perguntas comuns

- **O código de um celular funciona em qualquer conta?** Não. Cada conta gera um QR code próprio. O código do app só vale para a conta cujo QR code foi escaneado.
- **Dá para usar em qualquer aparelho?** Sim, desde que o app autenticador desse aparelho tenha escaneado o QR code da conta (dá para escanear o mesmo QR em mais de um celular na hora de ativar).
- **Perdeu o celular?** No Supabase: Authentication → Users → (pessoa) → remova o fator MFA.

## Moderação das denúncias

No Supabase, abra **Table Editor → reports_painel**. Lá aparece cada denúncia com o motivo, o texto do post, o autor, quem denunciou e quantas denúncias o post já tem. Para remover um post, abra **Table Editor → posts**, encontre o post pelo `post_id` e apague a linha. Para marcar a denúncia como resolvida, mude o `status` em **reports** para `analisada` ou `removida`.

## Personalizar

- **Cores:** altere `--accent` no começo do CSS. O vermelho da marca é `#a20e0c`.
- **Tema padrão:** troque `data-theme="dark"` por `data-theme="light"` na tag `<html>`.
- **Emojis:** edite as listas `EMOJI_TOP` (destaques) e `EMOJIS` no JavaScript.
- **Textos:** todos os textos ficam no objeto `I18N`, com `pt` e `en`.

## Limites do plano gratuito

O plano grátis do Supabase oferece 500 MB de banco, 1 GB de arquivos (fotos e capas) e 50 mil usuários ativos por mês. Projetos sem nenhum acesso por 7 dias são pausados, mas dá para reativar com um clique no painel.
