## Email Configuration

The application supports sending emails via an SMTP service. The same configuration parameters apply to both the **Server** and **Ingestor** components, but they **must be defined in the respective `.env` files** for each service.

### Configuration Parameters

| Parameter          | Type    | Description                                                                           |
| ------------------ | ------- | ------------------------------------------------------------------------------------- |
| **send_email**     | boolean | Enable or disable email sending. Set to `true` to allow email sending.                |
| **smtp_service**   | string  | Name of the SMTP service provider (e.g., gmail, custom).                   |
| **smtp_server**    | string  | Hostname or IP address of the SMTP server (e.g., `smtp.gmail.com`).                   |
| **smtp_port**      | int     | Port number for the SMTP server. Common ports: 25 (non-secure), 465 (SSL), 587 (TLS). |
| **smtp_secure**    | boolean | Enable secure connection (SSL/TLS) for SMTP communication.                            |
| **smtp_username**  | string  | Username for authenticating with the SMTP server (usually the email address).         |
| **smtp_password**  | string  | Password or app-specific password for the SMTP account. Keep confidential.            |
| **smtp_fromemail** | string  | Email address that will appear as the sender.                                         |
| **smtp_fromname**  | string  | Display name for the sender.                                                          |

---

### Example `.env` Entries

```env
# Enable email sending
send_email=true

# SMTP provider and server details
smtp_service=gmail
smtp_server=smtp.gmail.com
smtp_port=587
smtp_secure=true

# SMTP authentication
smtp_username=your_email@gmail.com
smtp_password=your_app_password

# Sender details
smtp_fromemail=your_email@gmail.com
smtp_fromname=Your Name
```

> **Note:** These entries should be added to both the **Server `.env`** and **Ingestor `.env`** files.

---

### Additional Notes

1. **Security:** Ensure `SMTP_SECURE` is enabled if your SMTP server supports SSL/TLS.
2. **App Passwords:** Some providers (e.g., Gmail) require generating an **App Password** for SMTP authentication.
3. **Testing:** Test email sending in a safe environment before production to avoid accidental emails.

---
