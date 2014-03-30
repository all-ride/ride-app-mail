## Configuration

You have optional 2 parameters which are used by the mail library:

* __mail.debug__
Make sure all outgoing mail is sent to the same recipient by setting the address in this parameter.
* __mail.from__
You can set the default from address in this variable.
When your code does not set a from address to the message, this value will be used.

## Send A Plain Text Mail

    <?php

    use ride\library\mail\transport\Transport;

    function foo(Transport $transport) {
        $message = $transport->createMessage();
        $message->setFrom('me@domain.com');
        $message->setTo('recipient@domain.com');
        $message->setSubject('subject');
        $message->setMessage('Your message');

        $transport->send($message);
    }

## Send A HTML Mail

    <?php

    use ride\library\mail\transport\Transport;
    use ride\library\mail\MimePart;

    function foo(Transport $transport) {
        $htmlMessage = '<html><<body><p>Your message</p></body></html>';
        $plainMessage = 'Your message';

        $message = $transport->createMessage();
        $message->setFrom('me@domain.com');
        $message->setTo('recipient@domain.com');
        $message->setSubject('subject');

        $message->setIsHtmlMessage(true);
        $message->setMessage($htmlMessage);

        // override the stripped HTML message with your own plain text version
        $message->addPart(new MimePart($plainMessage, 'text/plain'), MailMessage::PART_ALTERNATIVE);

        $transport->send($message);
    }

## Adding Attachments

    <?php

    use ride\library\mail\transport\Transport;
    use ride\library\system\file\browser\FileBrowser;

    function foo(Transport $transport, FileBrowser $fileBrowser) {
        $htmlMessage = '<html><<body><p>Your message</p></body></html>';
        $plainMessage = 'Your message';

        $message = $transport->createMessage();
        $message->setFrom('me@domain.com');
        $message->setTo('recipient@domain.com');
        $message->setSubject('subject');
        $message->setMessage('message');

        $file = $fileBrowser->getFile('data/text.txt');
        $mime = 'text/plain';

        $message->addAttachment($file, $mime);

        $transport->send($message);
    }
