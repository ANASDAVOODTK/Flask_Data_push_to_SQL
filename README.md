# Flask_Data_push_to_SQL
How to push data to mysql using Flask

<ol><li>Open PyCharm, create new Python file name app.python and type the below code into your app.python file.</li></ol>


<div class="wp-block-syntaxhighlighter-code "><pre class="brush: plain; title: ; notranslate" title="">
from flask import Flask

app = Flask(__name__)

@app.route('/', methods=&#91;'GET', 'POST'])
def index():
    return "Hello Nuclear Geeks"

if __name__ == '__main__':
    app.run()

</pre></div>


<ol start="2"><li>If you’ve gone through&nbsp;<a href="https://thenucleargeeks.com/2019/02/15/python-web-services-using-flask/" target="_blank" rel="noreferrer noopener">Python web services using Flask</a>&nbsp;you would understand 100% of the above code. Simply we’re routing out request and displaying “Hello Nuclear Geeks”, On running the following program type <a href="http://127.0.0.1:5000/" rel="nofollow">http://127.0.0.1:5000/</a> on your browser to see the output!! “Hello Nuclear Geeks”</li><li>Now you need to create a simple HTML page with two text field First Name, Last Name and submit button. To do this create a folder named Templates inside it create a file index.html and copy the below code.</li></ol>


<div class="wp-block-syntaxhighlighter-code "><pre class="brush: plain; title: ; notranslate" title="">
&lt;HTML&gt;
&lt;BODY bgcolor=&quot;cyan&quot;&gt;
&lt;form method=&quot;POST&quot; action=&quot;&quot;&gt;
    &lt;center&gt;
    &lt;H1&gt;Enter your details &lt;/H1&gt; &lt;br&gt;
    First Name &lt;input type = &quot;text&quot; name= &quot;fname&quot; /&gt; &lt;br&gt;
    Last Name &lt;input type = &quot;text&quot; name = &quot;lname&quot; /&gt; &lt;br&gt;
    &lt;input type = &quot;submit&quot;&gt;
    &lt;/center&gt;
&lt;/form&gt;
&lt;/BODY&gt;
&lt;/HTML&gt;

</pre></div>


<ol start="4"><li>Modify our app.python file and add the below code in it.</li></ol>


<div class="wp-block-syntaxhighlighter-code "><pre class="brush: plain; title: ; notranslate" title="">
from flask import Flask, render_template

app = Flask(__name__)

@app.route('/', methods=&#91;'GET', 'POST'])
def index():
    return render_template('index.html')

if __name__ == '__main__':
    app.run()

</pre></div>


<ol start="5"><li>Upon running the above code you must get the page as below.</li></ol>



<figure class="wp-block-image"><img src="https://process.filestackapi.com/cache=expiry:max/o3O949YQweVAw0ntQ8No" alt="screen-shot-2018-10-10-at-5-22-08-pm.png" /></figure>



<ol start="6"><li>Now we’ve developed our form, the next step is database connection. To create a table use the below query:</li></ol>


<div class="wp-block-syntaxhighlighter-code "><pre class="brush: plain; title: ; notranslate" title="">
CREATE TABLE MyUsers ( firstname VARCHAR(30) NOT NULL,  lastname VARCHAR(30) NOT NULL);

</pre></div>


<ol start="7"><li>The above query will create a table in the Database with name MyUsers, now copy the following code and paste in app.python file.</li></ol>


<div class="wp-block-syntaxhighlighter-code "><pre class="brush: plain; title: ; notranslate" title="">
from flask import Flask, render_template, request
from flask_mysqldb import MySQL
app = Flask(__name__)


app.config&#91;'MYSQL_HOST'] = 'localhost'
app.config&#91;'MYSQL_USER'] = 'root'
app.config&#91;'MYSQL_PASSWORD'] = 'root'
app.config&#91;'MYSQL_DB'] = 'MyDB'

mysql = MySQL(app)


@app.route('/', methods=&#91;'GET', 'POST'])
def index():
    if request.method == "POST":
        details = request.form
        firstName = details&#91;'fname']
        lastName = details&#91;'lname']
        cur = mysql.connection.cursor()
        cur.execute("INSERT INTO MyUsers(firstName, lastName) VALUES (%s, %s)", (firstName, lastName))
        mysql.connection.commit()
        cur.close()
        return 'success'
    return render_template('index.html')


if __name__ == '__main__':
    app.run()

</pre></div>


<ol start="8"><li>Pretty easy till now!!!</li></ol>



<p>app.config[‘MYSQL_HOST’] = ‘localhost’<br>app.config[‘MYSQL_USER’] = ‘root’<br>app.config[‘MYSQL_PASSWORD’] = ‘root’<br>app.config[‘MYSQL_DB’] = ‘MyDB’</p>



<p>These lines represent the db configuration required for our Flask, the next line ‘mysql = MySQL(app)’ creates an instance which will provide us the access.</p>



<p>The lines ‘firstName = details[‘fname’]’ and ‘lastName = details[‘lname’]’ fetches the entered value in the HTML form.</p>



<p>Establishment of connection is done by ‘cur = mysql.connection.cursor()’ and execution of query by ‘cur.execute(“INSERT INTO MyUsers(firstName, lastName) VALUES (%s, %s)”, (firstName, lastName))’</p>



<p>Once the execution is done you can commit and close the connection<br>mysql.connection.commit()<br>cur.close()</p>



<ol start="9"><li>Heavy Breathing!!! we are all set to run…. Run the program enter the First Name = “Aditya” and Last Name= “Malviya” and tap on submit. You will see success being returned on the screen.</li><li>Open the database and run the following query..</li></ol>


<div class="wp-block-syntaxhighlighter-code "><pre class="brush: plain; title: ; notranslate" title="">
SELECT * FROM MyUsers;

</pre></div>


<ol start="11"><li>You will be seeing the following output…..</li></ol>


<div class="wp-block-syntaxhighlighter-code "><pre class="brush: plain; title: ; notranslate" title="">
&gt; mysql&gt; select * from MyUsers;

+-----------+----------+

| firstname | lastname |

+-----------+----------+

| Aditya    | Malviya  |

+-----------+----------+

</pre></div>


<p>1 row in set (0.00 sec)<br></p>



<p>Easy !!! This was all about MySQL connection using Flask. In the next tutorial we will be playing with multiple request and multiple tables….<br>Do Comment if you have any queries, I&#8217;ll be more than happy to help you.</p>
