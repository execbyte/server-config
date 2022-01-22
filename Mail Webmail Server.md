# Postfix
---

## Interactive terminal when SSH
```bash
$ export TERM=ansi
```

## Source
1. [afreshcloud.com](https://afreshcloud.com/sysadmin/mail-terminology-mta-mua-msa-mda-smtp-dkim-spf-dmarc)
2. [serverfault.com](https://serverfault.com/questions/982121/does-postfix-also-operate-as-mail-submission-agent-msa)
3. [microsoft.com](https://support.microsoft.com/en-us/office/what-are-imap-and-pop-ca2c5799-49f9-4079-aefe-ddca85d5b1c9)
4. [mailtrap.io](https://mailtrap.io/blog/mail-transfer-agent/)

![How email works](email.png)

## How email works?
- It's needs email-client called `MUA` ( Mail User Agent ). 
	- Desktop Apps: `Mozilla Thunderbird`, `Microsoft Outlook`.
	- Web Mail Apps: `Roundcube`, `SquirrelMail`.
	- Cloud SaaS Apps: `GMail`, `ProtonMail`, `Yahoo`. 
- It's needs servers called `Email Server`. 
	- This server also work as `MTA` ( Mail Transfer Agent ) that routes and delivers email.
	- Example: `Postfix`, `Microsoft Exchange`.
- It's needs `MSA` ( Mail Submission Agent ).
	- This perform email error detection before an email deliver to 	`MTA`.
	- Many `MTA` perform the function as `MSA` as well.
	- Different vendors have different needs and structure.
	- Example: `Postfix` - with special configuration.
- It's needs `MDA` ( Mail Delivery Agent ).
	- This is the last server which will store email to receiver's mailbox ( `Email Server` ).
	- Example: `Dovecot`.
- It's needs `Protocol` ( rules/regulations ).
	- `SMTP` ( Simple Mail Transfer Protocol ) as the rule of email-message, delivery, etc.
	- `IMAP/POP3` which `MUA` uses to receive email from mailbox ( `Email Server` )
		- `IMAP`: Internet Message Access Protocol.
		- `POP3`: Post Office Protocol. 
		- Both `IMAP/` & `POP3` are the most popular protocol widely used.
		- Example: `Dovecot`. Yes, this one support both as `MDA` & `IMAP/POP3`.

## What is the need to build a local mail server?
1. `Email Server`, `MSA`, `MTA` : `Postfix`.
2. `MDA`, `IMAP` : `Dovecot`.
3. `MUA` : `Roundcube`.
4. Operating System: Debian 10 ( Buster ).

#### Postfix
