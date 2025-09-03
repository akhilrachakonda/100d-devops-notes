## Question
Create a user **kareem** with a **non-interactive shell** on **App Server 2**.

## My Answer (steps & commands)
```bash
ssh steve@stapp02
sudo useradd -s /sbin/nologin kareem || sudo useradd -s /bin/false kareem
grep '^kareem:' /etc/passwd
