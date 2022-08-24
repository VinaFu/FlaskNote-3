# FlaskNote-3

1) forms.py

      (build many and why?)
      (waht's the difference between validator and import?)
      (length and max?)

      from flask_wtf import FlaskForm
      from wtforms import StringField
      from wtforms.validators import DataRequired, Length, Email

      class RegistrationForm(FlaskForm):
          username = StringField("Username",
              validators=[DataRequired(), Length(min=2, max=20)])
          email = StringField("Email",
              validators=[DataRequired(), Email()])

2) flaskblog.py

1- add secret key?



2- && ternimal:
vinafu@Vinas-MacBook-Pro Flask_Blog % python3
Python 3.10.6 (v3.10.6:9c7b4bd164, Aug  1 2022, 17:13:48) [Clang 13.0.0 (clang-1300.0.29.30)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> import secrets
>>> secrets.token_hex(16)
'819d9ba658d1a3bcf9977c8a876c8b96'


3- from forms import RegistrationForm, LoginForm

4- 写完注册表格3）后，检验输入是否准确，上面importflash


3) register.html
form - POST!!
inside ....

anothe dive "already have an account?"

register.html: form is different

4) login.html




