Ruby on Rails using Puma web server on Heroku
=============================================

Sample Ruby on Rails application using Puma web server deployed at http://puma-demo.herokuapp.com.

On 23 January 2015 Heroku announced Puma as recommended Ruby web server. From [Heroku Changelog](https://devcenter.heroku.com/changelog-items/594):

> Heroku now recommends using the Puma webserver. The previously recommended webserver, Unicorn, is susceptible to slow client attacks. If you are using Unicorn on Heroku please migrate to Puma. For more information read Deploying Rails Applications with the Puma Web Server.

Optimal configuration:

* min puma threads == max puma threads
* \# of puma threads == # of db connections in ActiveRecord connection pool
* db connection limit (e.g. 20 for Hobby Dev) > puma processes * puma workers (e.g. 10 == 2 workers * 5 threads)

For 2 workers, 5 threads and AR connection pool size of 5, 10 db connections are established under stress load:

    ab -c 10 -n 500 http://puma-demo.herokuapp.com/

    heroku pg:status

    === HEROKU_POSTGRESQL_WHITE_URL (DATABASE_URL)
    Plan:        Hobba-dev
    Status:      Available
    Connections: 10/20
    PG Version:  9.3.3
    ...