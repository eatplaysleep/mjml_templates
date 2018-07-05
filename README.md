# mjml_templates

**CURRENT DIRECTORY IS `SaaS_Talent`**

## Install MJML Locally

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
## Usage

The templates are structured with the `index.mjml` as the primary file. The `index.mjml` includes: `attributes.mjml` `header.mjml` and `footer.mjml`.

The include files consist of the following:
    `attributes.mjml`: All the font, text, and other attributes/classes necessary to render the file correctly.
    `header.mjml`: The formatting for the header, which contains the logo and social network icons.
    `footer.mjml`: The footer logo, tag line, and unsubscribe link.

In order to properly render an email any variables contained in any of the files must be properly replaced. Variables are indicated by the use of `{{var:variable_name}}`. Variables in the base files are as follows:
    #### index.mjml
        `{{var:email_preview}}`: Provide a brief bit of text to be used in the email preview. For example, on a 'Welcome' email, the value for the variable could be `Welcome to Influential!`.      
        `{{var:msg_path}}`: Provide the path for the message to be used in the body of the email. For example, for the `msg_alert_content.mjml` message you would provide `./msg_alert_content.mjml` which would result in the content of that particular template being generated in the body of `index.mjml`.

    #### footer.mjml
        `{{var:unsub_link}}`: Provide the necessary data to properly generate an unsubscribe link. This variable is a natively supported variable when using MailJet. If no value is provided then the default value configured in MailJet will be used.

    #### msg_{template}.mjml
        Each message may use various different variables to fulfill the needs of the message. All variables will be in the `{{var:variable_name}}` format.

        Common variables include:
        `{{var:greeting}}`: The name/salutation to be used in the greeting. Typically the user's nickname.
        `{{var:user.name}}`: The full name of a user.
        `{{var:user.nickname}}`: The preferred nickname of a user, such as their first name.
        `{{var:email}}`: A user's email address.

**Remember that prior to rendering a message the message body in `index.mjml` must be compiled with the proper template path!**

### Render MJML to HTML

```bash
mjml index.mjml
```
This will output a HTML file called `index.html` in the root directory.
Input can also be a directory itself.

### Validate MJML

```bash
$> mjml -v index.mjml
```

It will log validation errors. If there are errors, exits with code 1. Otherwise, exits with code 0.

### Render and redirect the result to stdout

```bash
mjml -s index.mjml

# or

mjml --stdout index.mjml
```

### Minify and beautify the output HTML

```bash
$> mjml index.mjml --config.beautify true --config.minify false
```

These are the default options.

### Log error stack

```bash
$> mjml index.mjml --config.stack true
```

### Render and redirect the result to a file

```bash
mjml index.mjml -o email_output.html

# or

mjml index.mjml --output email_output.html
```

You can output the resulting email responsive HTML in a file.
If the output file does not exist it will be created, but output directories must already exist.
If output is a directory, output file(s) will be `output/input-file-name.html`

### Set the validation rule to `skip` so that the file is rendered without being validated.

```bash
mjml -l skip -r index.mjml
```

### Watch changes on a file

If you like live-coding, you might want to use the `-w` option that enables you to re-render your file every time you save it.
It can be time-saving when you can just split you screen and see the HTML output modified when you modify your MJML.

Of course, the `-w` option can be used with an `--output` option too.

```bash
mjml -w [file_name]

# or

mjml --watch [file_name]
```
