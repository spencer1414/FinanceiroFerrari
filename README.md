# OrganizOne (Multi-loja) — pronto para rodar

## 1) Requisitos
- Node 18+
- Conta Supabase

## 2) Criar projeto no Supabase
1. Crie um projeto no Supabase
2. Vá em **SQL Editor** e rode o arquivo: `supabase/sql/01_schema.sql`
3. Crie o bucket no Storage:
   - Nome: `organizone-files`
   - Público: **OFF**

## 3) Variáveis de ambiente
Crie um arquivo `.env.local` na raiz (copie do `.env.example`):

- `NEXT_PUBLIC_SUPABASE_URL`
- `NEXT_PUBLIC_SUPABASE_ANON_KEY`
- `SUPABASE_SERVICE_ROLE_KEY` (Settings → API → service_role)

> Importante: `SUPABASE_SERVICE_ROLE_KEY` é **server-only**. Nunca exponha no front.

## 4) Instalar e rodar
```bash
npm install
npm run dev
```
Abra `http://localhost:3000`

## 5) Primeiro admin
1) Crie um usuário em **Auth → Users** (email/senha)
2) Pegue o `id` do usuário e crie o profile como admin:

```sql
insert into public.profiles(user_id, role, store_id, name)
values ('SEU_USER_ID', 'admin', null, 'Admin');
```

Depois logue no sistema e use o menu Admin para cadastrar lojas e criar usuários.

## 6) Pastas de anexos
- Boleto/Nota: `bills/<store_id>/<payable_id>/...`
- Comprovante: `receipts/<store_id>/<payable_id>/...`

## 7) Fluxo do sistema
- Loja cadastra contas
- Sistema classifica vencimentos (20/15/10/5/hoje)
- Ao marcar como paga, anexa comprovante
- Admin vê tudo, exporta excel e gera notificações
