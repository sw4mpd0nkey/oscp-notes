## Union SQL Injection

Able to see if its vulnerable AND how many columns there are in response by putting the following and incrementing the number by 1 until you recieve an error

> ' ORDER BY 3-- //

Once you know how many cols are available you can start trying to pull out data using union select, remember to not go over your col limit.

> %' UNION SELEXT database(), user(), @@version, null, null -- //