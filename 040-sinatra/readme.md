#Build Podcast ep 040 - Sinatra
[Screencast link](http://build-podcast.com/sinatra/)

[Sinatra](http://www.sinatrarb.com/) is a simple domain-specific language that is minimalistic. In this episode, we will explore the basics of Sinatra such as routing, templates, sessions, errors with a simple project based on [Prisoner's Dilemma](http://en.wikipedia.org/wiki/Prisoner%27s_dilemma)!

version: 1.4.2

#Background on Sinatra 

1. [Main website](http://www.sinatrarb.com/)
1. [Documentation](http://www.sinatrarb.com/documentation.html)
2. [Github](https://github.com/sinatra/sinatra)

#Things to learn with Sinatra 

##1. install
1. Install with rubygems:

    ```
    gem install sinatra
    ```
2. in an empty project folder, create a file `app.rb` with the following code:

    ```
    require 'sinatra'
        get '/' do    "Hello, world!"    end
    ```
3. start the server in the command line and visit [localhost:4567](http://localhost:4567)

    ```
    $ ruby app.rb
    ```

4. Get info about the connection in the command line:

    ```
    $ curl -v http://localhost:4567
    $ curl -X POST -d 'user=myname' http://localhost:4567
    ```

##2. basics

1. routing with paths in `app.rb`

    ```
    require 'sinatra'

    get '/decision' do
      "What's the decision?"
    end
    ```

1. routes with wildcards in `app.rb`

    ```
    require 'sinatra'

    get '/*' do
    "You passed in #{params[:splat]}"
    end
    ```
    
    or with 2 wildcards
    
    ```
    require 'sinatra'

    get '/say/*/to/*' do
    "You passed in #{params[:splat]}"
    end
    ```
    
    or
    
    ```
    require 'sinatra'

    get '/say/*/to/*' do |user1, user2|
        "You passed in #{user1} and #{user2}"
    end
    ```

1. routing with parameters in `app.rb`

    ```
    require 'sinatra'

    get '/:decision' do
      "Your decision: #{params[:decision]}"
    end
    ```
    or with options
    
    ```
    require 'sinatra'

    get '/user/:first/?:last?' do
      "Hello #{params[:first]} #{params[:last]}"
    end
    ```

1. redirect with status code in `app.rb`

    ```
    require 'sinatra'

    get '/sinatra' do
      redirect 'http://www.sinatrarb.com/', 301
    end
    ```
1. static files. create a file `public/help.html` with `app.rb` as:

    ```
    require 'sinatra'
    
    get '/help.html' do
      'Documentation on Prisoner\'s Dilehma'
    end
    ```
    
    and the file `help.html`
    
    ```
    <!doctype html>
    <html lang="en">
    <head>
      <meta charset="UTF-8">
      <title>Prisoner's Dilehma</title>
    </head>
    <body>
    
      <h1>Prisoner's Dilehma</h1>
    
      <table border=1>
        <tr>
          <th></th>
          <th>B (silent)</th>
          <th>B (betrays)</th>
        </tr>
        <tr>
          <th>A (silent)</th>
          <td>1 year</td>
          <td>A : 3 years <br>  B : Free</td>
        </tr>
        <tr>
          <th>A (betrays)</th>
          <td>A : Free <br> B : 3 years</td>
          <td>2 years</td>
        </tr>
      </table>
    
    </body>
    </html>
    ```
1. template or view:

    - view file `views/decision.erb`:

        ```
        <!doctype html>
        <html lang="en">
        <head>
          <meta charset="UTF-8">
          <title>Decision</title>
        </head>
        <body>
        
          <p>A's decision:</p>
          <p>B's decision:</p>
        
        </body>
        </html>
        ```
    - in `app.rb`
    
        ```
        require 'sinatra'
        
        get '/decision' do
          erb :decision
        end    
        ```

1. template or view with data:

    - view file `views/decision.erb`:

        ```
        ...
        <p>A's decision: <strong><%= @a_decision %></strong></p>
        <p>B's decision: <strong><%= @b_decision %></strong></p>
        ...
        ```
    - in `app.rb`
    
        ```
        require 'sinatra'

        get '/decision' do
          @a_decision = "silent"
          @b_decision = "betray"
          erb :decision
        end 
        ```
1. filters in `app.rb`

    ```
    require 'sinatra'

    before do
      @heading = "Prisoner\'s Dilehma!"
    end
    ...
    ```
1. sessions in `app.rb`

    ```
    require 'sinatra'

    configure do
      enable :sessions
    end
    
    ...
    
    get '/set' do
      session[:now] = Time.now
      "Current session value is #{session[:now]}."
    end
    
    get '/fetch' do
      "Session value is #{session[:now]} and current time is #{Time.now}."
    end
    
    get '/clear' do
      session.clear
      redirect '/fetch'
    end 
    
    ```


##3. project - prisoner's dilehma

in `app.rb`:

```
require 'sinatra'

before do
  @heading = "Prisoner\'s Dilehma!"
  @decision = ["silent", "betray"]
end

get '/decision/:type' do

  @a_decision = params[:type].to_s
  @b_decision = @decision.sample.to_s
  @result = ''

  if @a_decision ==  @b_decision
    if @a_decision == 'silent'
      @result = 'Both get 1 year'
    else
      @result = 'Both get 2 years'
    end
  else
    if @a_decision == 'silent'
      @result = 'A gets 3 years, B is free!'
    else
      @result = 'A is free!'
    end
  end

  erb :decision

end
```


#More resources on the project - Prisoner's Dilehma

1. [Prisoner's Dilehma](http://en.wikipedia.org/wiki/Prisoner's_dilemma)
2. [Nash Equilibrium](http://en.wikipedia.org/wiki/Nash_equilibrium)
3. [Game Theory](http://en.wikipedia.org/wiki/Game_theory)

#More Resources on Sinatra 
1. [Sinatra book](http://sinatra-book.gittr.com/)
2. [Sinatra recipes](https://github.com/sinatra/sinatra-recipes)
3. [Simple Sinatra](https://tutsplus.com/course/simple-sinatra/)

#Related Build Podcast episodes

1. [Rails](http://build-podcast.com/rails/)
2. [package-managers](http://build-podcast.com/package-managers/)

#Build Link of this episode
[Markana](http://marakana.com/) with its stream of various [video talks](http://marakana.com/techtv/about.html) and [youtube channel](http://www.youtube.com/user/MarakanaTechTV)
