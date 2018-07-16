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
* `{{param:email_preview}}`: Provide a brief bit of text to be used in the email preview. For example, on a 'Welcome' email, the value for the parameter could be `Welcome to Influential!`.      
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
In addition to parameters, MailGun natively supports *variables*. Variables differ from parameters in that *they should be printed in the HTML output*. Variables are user visible. Their replacement values should be provided in a seperate JSON object sent to MailGun via their API and per their [specs](https://documentation.mailgun.com/en/latest/user_manual.html#attaching-data-to-messages). When MailGun sends the email they will replace the variables in the HTML content with the values provided in the JSON object.

#### footer.mjml
        `{{var:unsub_link}}`: Provide the necessary data to properly generate an unsubscribe link. This variable is a **natively supported variable when using MailJet.** If no value is provided then the default value configured in MailJet will be used.
        
#### Other Common variables include:
* `{{var:greeting}}`: The name/salutation to be used in the greeting. Typically the user's nickname.
* `{{var:user.name}}`: The full name of a user.
* `{{var:user.nickname}}`: The preferred nickname of a user, such as their first name.
* `{{var:email}}`: A user's email address.

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
