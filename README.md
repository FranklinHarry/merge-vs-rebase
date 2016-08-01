# Rebasing vs. Merging on Github

I had trouble answering ["why do you think [a branch behind master without conflicts] should be rebased?"][rebase-question] in clear terms so I created this repository as an example.

[rebase-question]: https://github.com/nevir/rubocop-rspec/pull/124#issuecomment-235739322

NOTE: This repo only touches on one clear difference. I'm not saying this is an exhaustive explanation and I'm not necessarily trying to argue that one option is better than the other.

---

## Comparison

In this repo I created 7 branches where I added one empty file in each branch. The first three branches were merged in succession without any rebasing. The last four branches were rebased on top of `origin/master` before being merged. Here is what the commit log looks like on github:

![github commit log](https://cloud.githubusercontent.com/assets/2085622/17282908/992e44b4-575f-11e6-8a76-56cfa112254d.png)

* Note how the commits and the merges are clustered together which generally makes it a bit more confusing what the master branch looked like at each point in time.

* Note how the rebased commits are grouped with their respective merges

Here is a graphical git client which includes some visualization of what the branching looks like in the git history:

![visual branching](https://cloud.githubusercontent.com/assets/2085622/17282977/69344fb4-5760-11e6-8a01-36c79587fdf9.png)

I think the history with rebased commits seems much simpler.

---

Steps involved in producing this repo:

## Setup repo with one root commit:

```sh
git init
git status .
git touch file1
git status .
git commit -m "Add file1"
git remote add origin git@github.com:backus/merge-vs-rebase.git
git push -u origin master
```

## Create three branches which will all be merged without rebasing

each of these branches are merged on Github in succession. They were all based on the root commit

```sh
git checkout -b branch1
git touch file2
git commit -m "Add file2"
git push -u origin branch1
git checkout master
```

```sh
git checkout -b branch2
git touch file3
git commit -m "Add file3"
git push -u origin branch2
git checkout master
git log
```

```sh
git checkout -b branch3
git touch file4
git commit -m "Add file4"
git push -u origin branch3
git checkout master
```

## Create four branches which are each be rebased onto origin/master before merging

Since these commits are rebased using `origin/master` their base commit is the tip of `origin/master`

```sh
git checkout -b branch4
git touch file5
git commit -m "Add file5"
git push -u origin branch4
git checkout master
```

```sh
git checkout -b branch5
git touch file6
git commit -m "Add file6"
git push -u origin branch5
git checkout master
```

```sh
git checkout -b branch6
git touch file7
git commit -m "Add file7"
git push -u origin branch6
git checkout master
```

### Rebasing and force pushing each branch

(Github merges happen between each rebase)

```sh
git checkout branch4
git fetch
git rebase origin/master
git push --force-with-lease
```

```sh
git checkout branch5
git fetch
git rebase origin/master
git push --force-with-lease
```

```sh
git checkout branch6
git fetch
git rebase origin/master
git push --force-with-lease
```

```sh
git checkout -b branch7
git touch file8
git commit -m "Add file8"
git push -u origin branch7
git checkout master
```
