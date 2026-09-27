# Reflection — Module 1

Getting the environment running took longer than I expected — most of my time went
into debugging Docker rather than reading code. The missing .env file and a stale
node_modules folder both caused confusing, unrelated-looking errors. I learned that
Docker's build cache can hide a fix even after you think you've solved the problem,
and that --force-recreate is sometimes needed on top of --build.

I haven't yet had time to trace a real request all the way through the code with
file:line references — my system map above is based on the architecture I can see
from the container names and ports, not from reading the actual route files yet.
I'll go deeper into the actual code for the next module.
