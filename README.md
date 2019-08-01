# svn_to_git_gmtsar

install git +svn

### Run

svn log -q svn://gmtserver.soest.hawaii.edu/GMTSAR | awk -F '|' '/^r/ {sub("^ ", "", $2); sub(" $", "", $2); print $2" = "$2" <"$2">"}' | sort -u > authors-transform.txt


### Edit the list to get correct username and emails related to github. Then create the git repository with the command below

git svn clone --trunk=/trunk --branches=/branches --tags=/tags svn://gmtserver.soest.hawaii.edu/GMTSAR --no-metadata -A authors-transform.txt ./temp

### Turn to bash, and rename the branches, and tags, etc

for t in $(git for-each-ref --format='%(refname:short)' refs/remotes/tags); do git tag ${t/tags\//} $t && git branch -D -r $t; done

for b in $(git for-each-ref --format='%(refname:short)' refs/remotes); do git branch $b refs/remotes/$b && git branch -D -r $b; done

### Delete or rename branches
git branch -D branch_name
git branch -m old_name new_name

### Then run
git remote add origin https://github.com/gmtsar/gmtsar.git
git push origin --all
 
### to create the repository





