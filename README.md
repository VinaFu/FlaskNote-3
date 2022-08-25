# FlaskNote-3

1) forms.py

      不用全部装在flaskblog里面，因为register和login里面都是表格，所以新建一个表格相关的python。
      flaskblog.py 定义了Registrationform（）所以在forms.py 里面定义。

      from flask_wtf import FlaskForm
      from wtforms import StringField, PasswordField, SubmitField, BooleanField
      from wtforms.validators import DataRequired, Length, Email, EqualTo

       
         1. 引入表格，WTF。Flask-wtf是一个用于表单处理、校验并提供csrf验证的功能的扩展库。
         2. wtforms引入StringField, PasswordField, SubmitField, BooleanField - 因为下文有要用到这些领域
         3. validator。要数据，数据长度，等于什么
         
         详解：https://flask.palletsprojects.com/en/2.2.x/patterns/wtforms/ 套路式模版
         中文版：https://dormousehole.readthedocs.io/en/latest/patterns/wtforms.html

      class RegistrationForm(FlaskForm):
          username = StringField("Username", validators=[DataRequired(), Length(min=2, max=20)])
          email = StringField("Email", validators=[DataRequired(), Email()])
          password = PasswordField("Password", validators=[DataRequired()])
          confirm_password = PasswordField("Confirm Password", validators=[DataRequired(), EqualTo("password")])
          submit = SubmitField("Sign Up")

      class LoginForm(FlaskForm):
          email = StringField("Email", validators=[DataRequired(), Email()])
          password = PasswordField("Password", validators=[DataRequired()])
          remember = BooleanField("Remember me")
          submit = SubmitField("Login")
              §


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

after 3) 4- 写完注册表格3）后，检验输入是否准确，上面importflash 
line36.
kibd of alart,但他有两个属性，还可以 成功！

紧随其后是返回主页面.32行记得补上 methods=["GET", "POST"]



3) register.html


8. 新打一个是否有账户？ 这边引用网页用：a href="{{ url_for('login') }}
9.  layout给了register一个flash：相当于alert，成功的话就返回/redirect到另一个网站。

                    详解：https://flask.palletsprojects.com/en/2.2.x/patterns/wtforms/ 套路式模版
                    中文版：https://dormousehole.readthedocs.io/en/latest/patterns/wtforms.html
              
              表格内部渲染范例（官网例子）：
                    {% macro render_field(field) %}
                          <dt>{{ field.label }} 填空的名字
                          <dd>{{ field(**kwargs)|safe }}
                          {% if field.errors %} 填空处要是有错误。field在form。py里面定义了
                            <ul class=errors>
                            {% for error in field.errors %} 
                              <li>{{ error }}</li>
                            {% endfor %}
                            </ul>
                          {% endif %}
                          </dd>
                     {% endmacro %}
               
               实际运用：
                  {% extends "layout.html" %}
                  {% block content %}
                      <div class="content-section">   上面都一样，引用layout的设置
                          <form method="POST" action="">    表格是界面给出来的信息，所以用  method = POST
                              {{ form.hidden_tag() }}     hidden tag？why？
                              <fieldset class="form-group">
                                  <legend class="border-bottom mb-4">Join Today</legend>
                                  <div class="form-group">
                                      {{ form.username.label(class="form-control-label") }}  填空的名字
                                      <br>
                                      {% if form.username.errors %}   如果出错
                                          {{ form.username(class="form-control form-control-lg is-invalid") }}
                                          <div class="invalid-feedback">
                                              {% for error in form.username.errors %}
                                                  <span>{{ error }}</span>
                                              {% endfor %}
                                          </div>
                                      {% else %}
                                          {{ form.username(class="form-control-lg") }}
                                      {% endif %}
                                  </div>
                                  email+password+confirm password 一样的
                              </fieldset>
                              
                              表格fieldset结束，下面是提交按钮：
                              <div class="form-group">
                                  {{ form.submit(class="btn btn-outline-info") }}
                              </div>
                          </form> 大表格结束
                      </div>
                      
                      上面整个是表格整体内容结束，额外问题：是否有帐户？
                      
                      <div class="border-top pt-3">
                          <small class="text-muted">
                              Already Have An Account? <a href="{{ url_for('login') }}" class="ml-2">Sign In</a>
                          </small>
                      </div>
                  {% endblock content%}
                 
            其他tips：                 
            -     render 渲染； template 模版
            -     legend 元素为 fieldset 元素定义标题（caption）
            -     
            -     <dl>
                  <dt>标题</dt>
                  <dd>内容1</dd>
                  <dd>内容2</dd>
                  </dl>
                  dl —— define list——定义列表
                  dt —— define list title —— 用于生成定义列表中各列表项的标题，重复使用可以定义多个列表项的标题。注意：其中不能包含 hx 元素。
                  dd —— define list define —— 用于生成定义列表各列表项的说明文字段，重复使用可以定义多个说明文字段。dd是对应dt的简短说明或解释。

4) login.html




