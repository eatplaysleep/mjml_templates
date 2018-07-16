# A few notes
1. The current working directory is `SaaS_Talent`.
2. The `sample_xxxxx.html` files can be opened in a browser to see what each template looks like at the time the file was generated. **If templates are modified, update the HTML output.**

## Structure of Repo/files

The templates are structured with the `index.mjml` as the primary file. The `index.mjml` includes: `attributes.mjml` `header.mjml` and `footer.mjml`.

The include files consist of the following:
    `attributes.mjml`: All the font, text, and other attributes/classes necessary to render the file correctly.
    `header.mjml`: The formatting for the header, which contains the logo and social network icons.
    `footer.mjml`: The footer logo, tag line, and unsubscribe link.

### Parameters
In order to properly render an email any **parameters** contained in any of the files must be properly replaced. Parameters are indicated by the use of `{{param:parameter_name}}`. Parameters are configurations/data that provide things like URLs, template names, and other configuration-based values that are used to generate the message and content. *Paramters are different from variables*. See information regarding variables below.
 
Parameters in the base files (excluding `footer.mjml`) are as follows:
#### index.mjml    
* `{{param:msg_path}}`: Provide the path for the message to be used in the body of the email. For example, for the `msg_alert_content.mjml` message you would provide `./msg_alert_content.mjml` which would result in the content of that particular template being generated in the body of `index.mjml`.

#### msg_{template}.mjml
 Each message may use various different parameters to fulfill the needs of the message. All parameters will be in the `{{param:paramter_name}}` format.

#### Other Common Parameters
The following are commonly found in various message templates or base file.
* `{{param.socialAccount.avatar}}`: The URL for the social account avatar as provided by Social Hoarder.
* `{{param.socialAccount.networkURL}}`: The network URL for the social account, such as the Facebook or Instagram URL.
* `{{param.post.mediaURL}}`: The URL for the media related to a post. Typically an S3 URL.
* `{{param.post.networkURL}}`: The network permalink for the actual post on the social network itself.
* `{{param.btn_cta}}`: The URL for any given button that provides the user call to action. For example, this may be an invitation URL for the user to click on in order to create an account.
 
### Variables 
In addition to parameters, MailJet natively supports *variables*. Variables differ from parameters in that *they should be printed in the HTML output*. Variables are user visible. Their replacement values should be provided in a seperate JSON object sent to MailJet via their API and per their [specs](https://dev.mailjet.com/template-language/reference/). 

When MailJet sends the email they will replace the variables in the HTML content with the values provided in the JSON object. See the [following link](https://dev.mailjet.com/template-language/mjml/) for additional information on the use of variables in MJML.

MailJet requires that the `TemplateLanguage` value be set to `true` per their documentation in order for rendering of variables to work.

Variables are in the following format: `{{var:varname:defaultvalue}}`. Notice than some variables below have a `space` as the default value so that if no value is provided the email will render a blank space.
 
#### Other Common variables include:
* `{{var:greeting:User}}`: The name/salutation to be used in the greeting. Typically the user's nickname.
* `{{var:username: }}`: The full name of a user as provided from the Customer micro service DB (`customers.contact.firstName`).
* `{{var:nickname: }}`: The preferred nickname of a user, such as their first name.
* `{{var:email: }}`: A user's email address.
* `{{var:socialaccountfirstname:Someone}}`: The first name of the owner of the social account as provided by Social Hoarder.
* `{{var:socialaccountnetwork:account}}`: The network of the social account referenced. I.e. Facebook or Instagram.
* `{{var:alertdescription:Custom Alert}}`: The name of the alert.
* `{{var:socialaccountusername:#}}`: The social account handle.
* `{{var:alerttype:change}}`: Type of alert being referenced, such as an `Increase` or `Decrease` alert.
* `{{var:alertparameter:some amount}}`: Increase/decrease of the follower amount that triggered the alert.
* `{{var:productname:Influential}}`: Name of the product referenced in the email. I.e. `Talent Watch`. 
* `{{var:companyname:a company}}`: Name of company being referenced in the email.
* `{{var:emailpreview:You have a notification from the Influential Network!}}`: The text that is visible in email clients prior to opening an email. If none is provided, the default will be set. *This variable is in the `index.mjml` file.*

##### Example of MailJet curl API
Assuming the following parameters/variables, as provided by the Notify service to the Email service, this is an example API call using variables.
```
param:recipient = dfuhriman@influential.co
param:recipient_name = Danny Fuhriman
param:btn_cta = https://www.influential.co

var:nickname = Danny
var:oldemail = dfuhriman1@influential.co
var:newemail = dfuhriman@influential.co
```
___
```
# This calls sends an email to the given recipient with vars and custom vars.
curl -s \
	-X POST \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3.1/send \
	-H 'Content-Type: application/json' \
	-d '{
		"Messages":[
			{
				"From": {
					"Email": "noreply@influential.co",
					"Name": "Influential Notifications"
				},
				"To": [
					{
					"Email": "dfuhriman@influential.co",
					"Name": "Danny Fuhriman"
					}
				],
				"HTMLPart": "<!doctype html><html xmlns="http://www.w3.org/1999/xhtml" xmlns:v="urn:schemas-microsoft-com:vml" xmlns:o="urn:schemas-microsoft-com:office:office"><head><title></title><!--[if !mso]><!-- --><meta http-equiv="X-UA-Compatible" content="IE=edge"><!--<![endif]--><meta http-equiv="Content-Type" content="text/html; charset=UTF-8"><meta name="viewport" content="width=device-width,initial-scale=1"><style type="text/css">#outlook a { padding:0; }
          .ReadMsgBody { width:100%; }
          .ExternalClass { width:100%; }
          .ExternalClass * { line-height:100%; }
          body { margin:0;padding:0;-webkit-text-size-adjust:100%;-ms-text-size-adjust:100%; }
          table, td { border-collapse:collapse;mso-table-lspace:0pt;mso-table-rspace:0pt; }
          img { border:0;height:auto;line-height:100%; outline:none;text-decoration:none;-ms-interpolation-mode:bicubic; }
          p { display:block;margin:13px 0; }</style><!--[if !mso]><!--><style type="text/css">@media only screen and (max-width:480px) {
            @-ms-viewport { width:320px; }
            @viewport { width:320px; }
          }</style><!--<![endif]--><!--[if mso]>
        <xml>
        <o:OfficeDocumentSettings>
          <o:AllowPNG/>
          <o:PixelsPerInch>96</o:PixelsPerInch>
        </o:OfficeDocumentSettings>
        </xml>
        <![endif]--><!--[if lte mso 11]>
        <style type="text/css">
          .outlook-group-fix { width:100% !important; }
        </style>
        <![endif]--><!--[if !mso]><!--><link href="https://fonts.googleapis.com/css?family=Ubuntu:300,400,500,700" rel="stylesheet" type="text/css"><style type="text/css">@import url(https://fonts.googleapis.com/css?family=Ubuntu:300,400,500,700);</style><!--<![endif]--><style type="text/css">@media only screen and (min-width:480px) {
        .mj-column-per-100 { width:100% !important; }
      }</style><style type="text/css"></style></head><body><div><!-- Header --><!--[if mso | IE]>
      <table
         align="center" border="0" cellpadding="0" cellspacing="0" style="width:600px;" width="600"
      >
        <tr>
          <td style="line-height:0px;font-size:0px;mso-line-height-rule:exactly;">
      <![endif]--><div style="Margin:0px auto;max-width:600px;"><table align="center" border="0" cellpadding="0" cellspacing="0" role="presentation" style="width:100%;"><tbody><tr><td style="direction:ltr;font-size:0px;padding:20px 0;padding-left:20px;text-align:center;vertical-align:middle;"><!--[if mso | IE]>
                  <table role="presentation" border="0" cellpadding="0" cellspacing="0">
                
        <tr>
      
            <td
               style="vertical-align:top;width:580px;"
            >
          <![endif]--><div class="mj-column-per-100 outlook-group-fix" style="font-size:13px;text-align:left;direction:ltr;display:inline-block;vertical-align:top;width:100%;"><table border="0" cellpadding="0" cellspacing="0" role="presentation" style="vertical-align:top;" width="100%"><tr><td align="center" style="font-size:0px;padding:10px 25px;word-break:break-word;"><table align="center" border="0" cellpadding="0" cellspacing="0" role="presentation" style="border-collapse:collapse;border-spacing:0px;"><tbody><tr><td style="width:530px;"><img height="auto" style="border:0;display:block;outline:none;text-decoration:none;width:100%;" width="530"></td></tr></tbody></table></td></tr></table></div><!--[if mso | IE]>
            </td>
          
        </tr>
      
                  </table>
                <![endif]--></td></tr></tbody></table></div><!--[if mso | IE]>
          </td>
        </tr>
      </table>
      <![endif]--><!-- Body --><!--[if mso | IE]>
      <table
         align="center" border="0" cellpadding="0" cellspacing="0" style="width:600px;" width="600"
      >
        <tr>
          <td style="line-height:0px;font-size:0px;mso-line-height-rule:exactly;">
      <![endif]--><div style="Margin:0px auto;max-width:600px;"><table align="center" border="0" cellpadding="0" cellspacing="0" role="presentation" style="width:100%;"><tbody><tr><td style="direction:ltr;font-size:0px;padding:20px 0;text-align:center;vertical-align:top;"><!--[if mso | IE]>
                  <table role="presentation" border="0" cellpadding="0" cellspacing="0">
                
        <tr>
      
            <td
               style="vertical-align:top;width:600px;"
            >
          <![endif]--><div class="mj-column-per-100 outlook-group-fix" style="font-size:13px;text-align:left;direction:ltr;display:inline-block;vertical-align:top;width:100%;"><table border="0" cellpadding="0" cellspacing="0" role="presentation" style="vertical-align:top;" width="100%"><tr><td align="left" style="font-size:0px;padding:10px 25px;word-break:break-word;"><div style="font-family:Ubuntu, Helvetica, Arial, sans-serif;font-size:13px;line-height:1;text-align:left;color:#000000;">Email Change/Update</div></td></tr></table></div><!--[if mso | IE]>
            </td>
          
        </tr>
      
                  </table>
                <![endif]--></td></tr></tbody></table></div><!--[if mso | IE]>
          </td>
        </tr>
      </table>
      
      <table
         align="center" border="0" cellpadding="0" cellspacing="0" style="width:600px;" width="600"
      >
        <tr>
          <td style="line-height:0px;font-size:0px;mso-line-height-rule:exactly;">
      <![endif]--><div style="Margin:0px auto;max-width:600px;"><table align="center" border="0" cellpadding="0" cellspacing="0" role="presentation" style="width:100%;"><tbody><tr><td style="direction:ltr;font-size:0px;padding:3px 0px 0px 0px;text-align:center;vertical-align:top;"><!--[if mso | IE]>
                  <table role="presentation" border="0" cellpadding="0" cellspacing="0">
                
        <tr>
      
            <td
               style="vertical-align:top;width:600px;"
            >
          <![endif]--><div class="mj-column-per-100 outlook-group-fix" style="font-size:13px;text-align:left;direction:ltr;display:inline-block;vertical-align:top;width:100%;"><table border="0" cellpadding="0" cellspacing="0" role="presentation" style="vertical-align:top;" width="100%"><tr><td align="left" style="font-size:0px;padding:10px 25px;word-break:break-word;"><table align="left" border="0" cellpadding="0" cellspacing="0" role="presentation" style="border-collapse:collapse;border-spacing:0px;"><tbody><tr><td style="width:99px;"><img height="auto" src="https://www.dropbox.com/s/w3e9hp835xmjelw/Gradient_Separator%403x.png?raw=1" style="border:0;display:block;outline:none;text-decoration:none;width:100%;" width="99"></td></tr></tbody></table></td></tr></table></div><!--[if mso | IE]>
            </td>
          
        </tr>
      
                  </table>
                <![endif]--></td></tr></tbody></table></div><!--[if mso | IE]>
          </td>
        </tr>
      </table>
      
    
        <table role="presentation" border="0" cellpadding="0" cellspacing="0"><tr><td height="17" style="vertical-align:top;height:17px;">
      
    <![endif]--><div style="height:17px;">&nbsp;</div><!--[if mso | IE]>
    
        </td></tr></table>
      
    
      <table
         align="center" border="0" cellpadding="0" cellspacing="0" style="width:600px;" width="600"
      >
        <tr>
          <td style="line-height:0px;font-size:0px;mso-line-height-rule:exactly;">
      <![endif]--><div style="Margin:0px auto;max-width:600px;"><table align="center" border="0" cellpadding="0" cellspacing="0" role="presentation" style="width:100%;"><tbody><tr><td style="direction:ltr;font-size:0px;padding:0px;text-align:center;vertical-align:top;"><!--[if mso | IE]>
                  <table role="presentation" border="0" cellpadding="0" cellspacing="0">
                
        <tr>
      
            <td
               style="vertical-align:top;width:600px;"
            >
          <![endif]--><div class="mj-column-per-100 outlook-group-fix" style="font-size:13px;text-align:left;direction:ltr;display:inline-block;vertical-align:top;width:100%;"><table border="0" cellpadding="0" cellspacing="0" role="presentation" style="vertical-align:top;" width="100%"><tr><td align="left" style="font-size:0px;padding:10px 25px;word-break:break-word;"><div style="font-family:Ubuntu, Helvetica, Arial, sans-serif;font-size:13px;line-height:1;text-align:left;color:#000000;"><p>Hey, {{var:nickname:User}}! We noticed you made a change to your email address recently.</p><p><b>Old Email</b><br>{{var:oldemail:null}}</p><p><b>New Email</b><br>{{var:newemail:null}}</p><p style="text-align:center">You can now log in with your new email.</p></div></td></tr><tr><td align="center" vertical-align="middle" style="font-size:0px;padding:0px 0px 0px 0px;word-break:break-word;"><table align="center" border="0" cellpadding="0" cellspacing="0" role="presentation" style="border-collapse:separate;line-height:100%;"><tr><td align="center" bgcolor="#414141" role="presentation" style="border:none;border-radius:3px;color:#ffffff;cursor:auto;padding:10px 25px;" valign="middle"><a href="https://www.influential.co" style="background:#414141;color:#ffffff;font-family:Ubuntu, Helvetica, Arial, sans-serif;font-size:13px;font-weight:normal;line-height:120%;Margin:0;text-decoration:none;text-transform:none;" target="_blank">Login</a></td></tr></table></td></tr><!-- <mj-text mj-class="disclaimer"><![CDATA[
      Or verify using this link:
      <a href="%7B%7Bparam%3Abtn_cta%7D%7D">{{param:btn_cta}}</a>
    ]]></mj-text> --><tr><td align="left" style="font-size:0px;padding:10px 25px;word-break:break-word;"><div style="font-family:Ubuntu, Helvetica, Arial, sans-serif;font-size:13px;line-height:1;text-align:left;color:#000000;"><p>If you did not request this change, please contact our team at <a href="mailto:support@influential.co?subject=Invalid%20email%20address%20change">support@influential.co</a>.</p></div></td></tr></table></div><!--[if mso | IE]>
            </td>
          
        </tr>
      
                  </table>
                <![endif]--></td></tr></tbody></table></div><!--[if mso | IE]>
          </td>
        </tr>
      </table>
      <![endif]--></div></body></html>",
				"TemplateLanguage": true,
				"Subject": "Email Changed/Update",
				"Variables": {
					"nickname": "Danny",
					"oldemail": "dfuhriman1@influential.co",
                    "newemail": "dfuhriman@influential.co"
				}
			}
		]
	}'
```
### footer.mjml <-- The exception!
MailJet supports certain variables natively. If the unsubscription link is configured then the following variable can be printed out in the HTML and will automatically be replaced. 
* `{{mj:unsub_link}}`: Provide the necessary data to properly generate an unsubscribe link. This variable is a **natively supported variable when using MailJet.** If no value is provided then the default value configured in MailJet will be used.
___

**Remember that prior to rendering a message the message body in `index.mjml` must be compiled with the proper template path parameter!**

___

## Installation Options

### 1. Use the MJML App (recommended)
The easiest method of developing with MJML is to use their native app. Simply download [the app](https://mjmlio.github.io/mjml-app/) and the repo and then open the project in the app. 

### 2. Install MJML Locally

Recommend installing and using MJML locally, in a project folder where you'll use MJML:
```bash
npm install mjml
```
In the folder where you installed MJML you can now run:
```bash
./node_modules/.bin/mjml [command]
```
To avoid typing `./node_modules/.bin/`, add it to your PATH:
```bash
export PATH="$PATH:./node_modules/.bin"
```
You can now run MJML directly, in that folder:
```bash
mjml [command]
```
#### Local Install Usage
##### Render MJML to HTML

```bash
mjml index.mjml
```
This will output a HTML file called `index.html` in the root directory.
Input can also be a directory itself.

##### Validate MJML

```bash
$> mjml -v index.mjml
```

It will log validation errors. If there are errors, exits with code 1. Otherwise, exits with code 0.

##### Render and redirect the result to stdout

```bash
mjml -s index.mjml

# or

mjml --stdout index.mjml
```

##### Minify and beautify the output HTML

```bash
$> mjml index.mjml --config.beautify true --config.minify false
```

These are the default options.

##### Log error stack

```bash
$> mjml index.mjml --config.stack true
```

##### Render and redirect the result to a file

```bash
mjml index.mjml -o email_output.html

# or

mjml index.mjml --output email_output.html
```

You can output the resulting email responsive HTML in a file.
If the output file does not exist it will be created, but output directories must already exist.
If output is a directory, output file(s) will be `output/input-file-name.html`

##### Set the validation rule to `skip` so that the file is rendered without being validated.

```bash
mjml -l skip -r index.mjml
```

##### Watch changes on a file

If you like live-coding, you might want to use the `-w` option that enables you to re-render your file every time you save it.
It can be time-saving when you can just split you screen and see the HTML output modified when you modify your MJML.

Of course, the `-w` option can be used with an `--output` option too.

```bash
mjml -w [file_name]

# or

mjml --watch [file_name]
```
