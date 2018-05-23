# swjava
java 项目
JAVAMAIL发送QQ邮箱

package com.sw.utils;

import java.util.Properties;

import javax.mail.Authenticator;
import javax.mail.Message;
import javax.mail.Message.RecipientType;
import javax.mail.MessagingException;
import javax.mail.PasswordAuthentication;
import javax.mail.Session;
import javax.mail.Transport;
import javax.mail.internet.AddressException;
import javax.mail.internet.InternetAddress;
import javax.mail.internet.MimeMessage;

/**
 * ClassName:邮件发送的工具类 <br/>
 * Function: TODO ADD FUNCTION. <br/>
 * Reason: TODO ADD REASON. <br/>
 * Date: 2018年5月22日 下午9:40:43 <br/>
 * 
 * @author wei.shao
 * @version
 * @since JDK 1.8
 * @see
 */
public class MailUtils {

	/**
	 * Description: 发送邮件的方法<br/>
	 * Date: 2018年5月22日 下午9:41:34 <br/>
	 * 
	 * @author wei.shao
	 * @param to
	 *            ：给谁发送邮件
	 * @param code
	 *            ：邮件的激活码
	 * @throws MessagingException
	 * @throws AddressException
	 */
	public static void sendMail(String to, String code) throws Exception {
		// 1.创建连接对象，记录邮箱的一些属性，连接到邮箱服务器
		Properties props = new Properties();
		// props.setProperty("host", value);
		
		//使用163或者qq给他人发送邮件，开始smtp服务，
//		props.setProperty("mail.host", "smtp.163.com");
//		props.setProperty("mail.smtp.auth", "true");
		props.setProperty("mail.transport.protocol", "smtp");
		//表示SMTP发送邮件，必须进行身份验证。
		props.setProperty("mail.smtp.auth", "true");
		//此处填写SMTP服务器
		props.setProperty("mail.smtp.host", "smtp.qq.com");
		//端口号，qq邮箱给出两个
		props.put("mail.smtp.port", "465");
		//指定在使用指定的套接字工厂时要连接的端口
		props.setProperty("mail.smtp.socketFactory.port", "465");
		//发送邮件协议
		props.setProperty("mail.transport.protocol", "smtp");
		//如果设置为扩展javax.net.ssl.SSLSocketFactory类的类，则该类将用于创建SMTP SSL套接字。
		props.setProperty("mail.smtp.socketFactory.class", "javax.net.ssl.SSLSocketFactory");
		                                               //构建授权信息，用于进行SMTP进行身份验证
		Session session = Session.getInstance(props, new Authenticator() {
			@Override
			protected PasswordAuthentication getPasswordAuthentication() {
//				return new PasswordAuthentication("service@sw.com", "123");// 给别的邮件发送消息的账户,分别要写账户和密码
				
				//填写自己的qq邮箱账户 和获取的授权码
				return new PasswordAuthentication("xxxxx@qq.com", "xxx");
			}
		});
		// 2.创建邮件对象
		Message message = new MimeMessage(session);
		// 2.1 设置发件人
//		message.setFrom(new InternetAddress("service@sw.com"));
		message.setFrom(new InternetAddress("xxx@qq.com"));
		// 2.2 设置收件人
		message.setRecipient(RecipientType.TO, new InternetAddress(to));
		// 2.3设置邮件的主题
		message.setSubject("来自邵炜的激活邮件");
		// 2.4设置邮件的正文
		message.setContent(
				"<h1>来自邵炜的激活邮件，激活请点击一下链接<h1><h3><a href='http://localhost:8080/regist_web/ActiveServlert?code=" + code
						+ "'>http://localhost:8080/regist_web/ActiveServlert?code=" + code + "</a></h3>",
				"text/html;charset=UTF-8");
		// 3.发送一封激活邮件
		Transport.send(message);
	}
}
