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
              



2) register.html

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
            -     a href="{{ url_for('login') }}" 用这个来引用网页。
            -     flash：相当于alert，成功的话就返回/redirect到另一个网站。但他有两个选项。
            -     <dl>
                  <dt>标题</dt>
                  <dd>内容1</dd>
                  <dd>内容2</dd>
                  </dl>
                  dl —— define list——定义列表
                  dt —— define list title —— 用于生成定义列表中各列表项的标题，重复使用可以定义多个列表项的标题。注意：其中不能包含 hx 元素。
                  dd —— define list define —— 用于生成定义列表各列表项的说明文字段，重复使用可以定义多个说明文字段。dd是对应dt的简短说明或解释。



2) flaskblog.py

            因为有重新换页面，所以flask部分增加：flash 和 redirect
            因为有新表格：from forms import RegistrationForm, LoginForm

            1. app.config["SECRET_KEY"]  是在干嘛 + 密码从terminal里面找出来
         && ternimal:
            vinafu@Vinas-MacBook-Pro Flask_Blog % python3
            Python 3.10.6 (v3.10.6:9c7b4bd164, Aug  1 2022, 17:13:48) [Clang 13.0.0 (clang-1300.0.29.30)] on darwin
            Type "help", "copyright", "credits" or "license" for more information.
            >>> import secrets
            >>> secrets.token_hex(16)
            '819d9ba658d1a3bcf9977c8a876c8b96'

     内部增加：
            编辑route，多加一个网页就要加一个“/register”
            同时还要注意加上 GET，POST 这两个methods：
            
            @app.route("/register", methods=["GET", "POST"])
            def register ():
                form = RegistrationForm()
                if form.validate_on_submit():
                    flash(f"Account created for {form.username.data}!","success")
                    return redirect(url_for('home')) 
                    // 这个return是输入成功就返回主页面
                return render_template('register.html',title="Register", form=form)
                // 这个return是页面渲染，外观的。

            @app.route("/login", methods=["GET", "POST"])
            def login ():
                form = LoginForm()
                if form.validate_on_submit():
                    if form.email.data == "admin@blog.com" and form.password.data == "password":
                        flash("You have been logged in!", "success")
                        return redirect(url_for('home'))
                    else:
                        flash("Login Unsuccessful. Please check username and password", "danger")
                return render_template('login.html',title="Login", form=form)


4) login.html
  
  基本上复制register内容即可。主要要的项，以及flash回哪里？
  多一个check。


