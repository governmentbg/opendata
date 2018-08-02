# Справки и SQL заявки

Тези заявки са били ползвани в различни моменти да се извадят справки за различна информация от базата на портала.

Предполага се, че SQL заявките се изпълняват от тук:

```
sudo -u postgres -i psql
```

И че в psql shell-а е установена връзка към базата данни:

```
\c opendata
```

## Списък с потребители и техните роли

```sql
COPY (
SELECT
  u.name,
  u.fullname,
  u.email,
  u.about,
  u.sysadmin,
  u.state,
  array_to_string(array(
    SELECT CONCAT(g.title, ' (', m.capacity, ')')
    FROM public.member m JOIN public.group g ON g.id = m.group_id
    WHERE g.state = 'active' AND m.state = 'active' AND m.table_id = u.id AND m.table_name = 'user'
  ), ', ') roles
FROM public.user u
WHERE u.state = 'active'
) TO '/tmp/opendata_users_with_roles.csv' DELIMITER ',' CSV HEADER;
```
