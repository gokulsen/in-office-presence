# Office Presence

## Supabase setup

Run this SQL in the Supabase SQL Editor before opening `index.html`:

```sql
create table if not exists public.attendance_calculator (
  id text primary key,
  baseline jsonb not null,
  weeks jsonb not null,
  plans jsonb not null,
  updated_at timestamptz not null default now()
);

alter table public.attendance_calculator enable row level security;

create policy "Allow public calculator read"
  on public.attendance_calculator for select
  to anon
  using (id = 'default');

create policy "Allow public calculator write"
  on public.attendance_calculator for insert
  to anon
  with check (id = 'default');

create policy "Allow public calculator update"
  on public.attendance_calculator for update
  to anon
  using (id = 'default')
  with check (id = 'default');
```

The page uses the browser-safe Supabase publishable key and stores one shared
`default` calculator row. Never put a Supabase service-role/secret key in this
file. The public policies above intentionally allow anyone with the page to
read or edit that shared calculator.