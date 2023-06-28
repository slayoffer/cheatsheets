### Correct flow

##### If you are going to change submodule code do it beforehand in submodule repo itself! 

##### Dont do it in main repo which uses this submodule!!!

```bash
# after it do
git submodule update --init

# then
git submodule update --remote

# then
git add . && git commit -am "update git spa submodule [skip ci]"

# and
git push origin dev
```