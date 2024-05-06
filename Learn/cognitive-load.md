The goal is to reduce at minimum indispensable/essential the cognitive load. Possibly try to reduce it at ZERO (0). 

Write code is simple. Everyone can do it.
Write maintanable code is harder. It's a fight against cognitive load.

## Things that adds to cognitive load on software development/debugging


#### function call in the param of another function for example:

funcA(funcB(z, l), x, y)

why adds cognitive load? because the result of the funcB will be stored in the mind and will be needed to be recalculated
every time. It can be reduced if the funcB is called with a better name, such as GetLastUsers(), so

funcA(GetLastUsers(z, l), x, y)


however is better if the result is stored in variable with a semantic name.

lastUsers = funcB(z, l );
funcA(lastUsers, x, y)

even better:
lastUsers = GetLastUsers(z, l );
doSomethingWithLastUsers(lastUsers, x, y)

#### naming things with different names from the domain names

for example, if the your domain has a Visit entity, if you call your variable different, for example 'Form', you will have cognitive load
since every time you will need to perform the mapping between 'forms' and associate it with 'Visit.
So call it with exatcly the domain semantic, even if iw would result in a longer name.

so the variable would be 'Visit'

