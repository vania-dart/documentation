---
sidebar_label: 'Mail'
sidebar_position: 13
---

# Mail

## Introduction

Effective communication is essential for maintaining client relationships, managing account recovery processes, sending notifications, etc. Vania simplifies this by integrating a robust mailing feature, leveraging the capabilities of the [Mailer Package](https://pub.dev/packages/mailer) to handle email sending effortlessly.

## Quick Start

### Configuration

First, configure your mail server settings in the `.env` file. Vania supports SMTP server or various other email providers too

```env
MAIL_MAILER="smtp"
MAIL_HOST="smtp-mail.outlook.com"
MAIL_PORT="587"
MAIL_ENCRYPTION=true
MAIL_USERNAME=vania.note.dart@gmail.com
MAIL_PASSWORD='armd hlco qzku unae'
MAIL_FROM_ADDRESS=vania.note.dart@gmail.com
MAIL_FROM_NAME="Note Vania"
MAIL_ACCESS_TOKEN=''
```

### Supported Drivers

Vania is compatible with multiple email drivers to suit your needs:

```text
smtp
gmail
gmailSaslXoauth2
gmailRelaySaslXoauth2
hotmail
mailgun
qq
yahoo
yandex
```

## Create a Mail Class

To send emails, create a Mail class that extends `Mailable`. Use the following command to generate a new Mail class:

```shell
vania make:mail recovery_password_mail
```

The generated class will be placed under `lib/app/mail/` in your project directory. Here's what the `RecoveryPasswordMail` class looks like:

```dart
class RecoveryPasswordMail extends Mailable {
  final String to;
  final String text;
  final String subject;

  const RecoveryPasswordMail({required this.to, required this.text, required this.subject});

  @override
  List<Attachment>? attachments() {
    return null;  // Define attachments if needed
  }

  @override
  Content content() {
    return Content(text: text);  // Main content of the email
  }

  @override
  Envelope envelope() {
    return Envelope(
      from: Address('info@Vania.com', 'Vania'),
      to: [Address(to)],
      subject: subject,
    );
  }
}
```

## Sending Emails

To send an email, simply call the `send()` method on an instance of your Mail class:

```dart
Future<Response> index() async {
  try {
    await RecoveryPasswordMail(
      to: 'client@gmail.com',
      text: 'Your password recovery code is 55625',
      subject: 'Password Recovery',
    ).send();
  } catch (e) {
    print('Error sending email: $e');
  }
  return Response.json({'message': 'Recovery password E-mail sent'});
}
```

With these configurations, Vania provides a flexible and powerful email solution to facilitate communication with your clients.
