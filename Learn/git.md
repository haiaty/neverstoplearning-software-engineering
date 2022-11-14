
### working with tags
-- see https://phoenixnap.com/kb/git-checkout-tag

mkdir git_experimentations
  
cd git_experimentations/
  
git init
 
 echo "source1" > source.js
 
ls

git add .

git commit -m "first commit"

git tag v1.0.0  --> create the tag that points to the latest commit, in this case "first commit"

git tag --> see all available tags

cat source.js

echo "sourcechanged" > source.js --> apply a change to the file and create then a second tag

cat source.js

git add .

git commit -m "second commit"

git tag v1.0.1  --> creates the second tag

git tag --> list tags

cat source.js

git checkout -b v1.0.0 v1.0.0   ---> checkout to tag in a new branch, in this case the branch name is v1.0.0 (first param) and the tag to be used is v1.0.0 (second parameter). why? for example if you need to rollback to a previous version of the code

git status

cat source.js

git checkout master ---> after fixed the problem and merget to master you can turn back to master with this command

git pull  ----> then pull the fixes
