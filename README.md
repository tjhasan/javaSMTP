This is the code to setup a SMTP Server in Java using TLS Authentication

Add the following dependency to your pom.xml file
```
<dependency>
    <groupId>com.sun.mail</groupId>
    <artifactId>javax.mail</artifactId>
    <version>1.5.5</version>
</dependency>
```

After you have done that, the following code snippet will allow you to setup the SMTP server.

```
import javax.mail.*;
import javax.mail.internet.InternetAddress;
import javax.mail.internet.MimeMessage;
import java.util.Properties;

public class App {
    public static void main(String[] args) {
       String username = "[Email goes here]";
       String password = "[app app password goes here]";
```
Remember, you need to generate a new app password by going to Gmail Account settings -> Security -> 2FA -> App passwords
```
       Properties p = new Properties();
       p.put("mail.smtp.host", "smtp.gmail.com");
       p.put("mail.smtp.port", "587");
       p.put("mail.smtp.auth", "true");
       p.put("mail.smtp.starttls.enable", "true");
       System.out.println("Setup server");

       Session session = Session.getInstance(p, new javax.mail.Authenticator(){
           protected PasswordAuthentication getPasswordAuthentication(){
               return new PasswordAuthentication(username,password);
           }//PassAuth
       });//mailAuth
       System.out.println("Creating email session");

       try 
       {
            Message msg = new MimeMessage(session);
            msg.setFrom(new InternetAddress("[senders email]"));
            msg.setRecipients(Message.RecipientType.TO, 
                    InternetAddress.parse("[string of email addresses to send to seperated by ',']")
                );
            msg.setSubject("[Subject line]");
            msg.setText("[email body]");

            System.out.println("Sending email");

            Transport.send(msg);

            System.out.println("Complete");
       }

       catch(Exception e)
       {
           e.printStackTrace();
       }

    }//main
}//class
```