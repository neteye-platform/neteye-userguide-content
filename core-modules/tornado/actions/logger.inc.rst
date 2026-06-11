.. _tornado-logger-executor:

Logger
~~~~~~

For troubleshooting purposes the LOGGER Action can be used to log all events that match a specific rule.
The Logger Executor behind this Action type logs received Actions: it simply outputs the whole
Action body to the standard `log <https://crates.io/crates/log>`__ at
the *info* level.
