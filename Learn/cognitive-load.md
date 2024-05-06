The goal is to reduce at minimum indispensable/essential the cognitive load. Possibly try to reduce it at ZERO (0). 

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

