# Lab 0 Git 实验报告

## 1. 实验目的

本次实验主要学习 Git 的基本使用方法，并通过实际操作理解 Git 在版本管理和多人协同开发中的作用。
实验主要包括以下内容：

- 学习 Git 的基本概念和基本操作；
- 完成 main.c 文件中的 TODO 部分并提交；
- 学习 Git 的暂存、提交、分支和合并机制；
- 创建 feature 分支，并分别在 main 和 feature 分支上进行修改；
- 人为制造一次合并冲突，并学习如何定位和解决冲突；
- 阅读 Git 相关资料，进一步理解规范化版本管理的重要性。

## 2. 文档中要求回答的问题

### 2.1 之前有过多人协同开发的经历吗？如果有，你们是使用什么方式分工协作的？

此前没有正式参与过大型多人软件开发，但在课程作业和小组项目中有过多人协作经历。

以前进行多人协作时，通常采用分工完成不同任务，然后通过文件传递、共享文档等方式进行整合。这种方式比较直观，但是当多人同时修改同一个文件时，容易出现版本不一致、修改内容互相覆盖等问题。

Git 可以很好地解决这一问题。不同成员可以在不同分支上进行开发，每个人的修改都会被记录下来，完成之后再通过 merge 等方式进行合并。如果不同分支修改了同一个位置，Git 会明确指出冲突位置，由开发者手动判断应该保留哪部分内容。

因此，与简单地通过文件传递相比，Git 能够更加清晰地记录每个人的修改过程，也方便在出现问题时查看和回退历史版本。

### 2.2 Git 为什么要设计 “暂存 — 提交” 两个步骤？

Git 将代码修改过程划分为工作区、暂存区和版本库，因此修改文件之后并不能直接成为一次 commit，而是需要经过 `git add` 和 `git commit` 两个步骤。

例如，我同时修改了 main.c 和 README.md，但这两个修改属于不同的任务。如果希望只提交 main.c 的修改，可以执行：

```
git add main.c
git commit -m "feat: update main.c"
```

这样就可以只把 main.c 的修改放入这次提交，而不会把 README.md 的修改一起提交。

因此，暂存区相当于在 “当前所有修改” 和 “下一次正式提交” 之间增加了一层选择机制。

这种设计有几个好处：

- 可以选择性地提交部分修改；
- 可以让一次 commit 只对应一个相对独立的任务；
- 方便查看和审查提交内容；
- 如果以后需要回退某一次修改，更容易定位具体的 commit；
- 在多人协作项目中，可以让提交历史更加清晰。

因此，我认为 “暂存 — 提交” 两个步骤并不是多余的操作，而是 Git 对版本管理进行精细控制的重要机制。

### 2.3 git branch 和 git branch -a 的区别是什么？

`git branch` 默认显示当前 Git 仓库中的本地分支。

例如：

```
git branch
```

可能显示：

```
* main
  feature
```

其中 `*` 表示当前所在的分支。

而：

```
git branch -a
```

会显示本地分支以及远程跟踪分支。

例如：

```
* main
  feature
  remotes/origin/main
  remotes/origin/feature
```

其中：
main、feature 是本地分支；
remotes/origin/main、remotes/origin/feature 是远程跟踪分支。

因此，二者的主要区别是：
`git branch` 主要查看本地分支，而 `git branch -a` 会同时显示本地分支和远程分支。

## 3. 完成 main.c 中 TODO 的实验过程

### 3.1 查看实验仓库

首先建立并进入实验仓库：

```
cd ~/testlab
```

通过 Git 查看当前仓库状态：

```
git status
```

确认当前位于 main 分支，并查看仓库中的文件。

### 3.2 修改 main.c

打开 main.c，按照实验要求完成其中的 TODO 部分。
本次实验中对程序进行了修改，使程序能够输出相应的字符串。

修改完成后，可以通过：

```
git diff
```

查看本次修改的具体内容。

### 3.3 编译和运行程序

按照实验文档中的方法尝试编译和运行：

```
make
./main
make clean
```

其中：
make 用于编译程序；
./main 用于运行生成的可执行文件；
make clean 用于删除编译过程中生成的目标文件。

通过这一过程，可以检查 main.c 的修改是否能够正常编译。

### 3.4 提交 main.c 的修改

完成代码修改后，将文件加入暂存区：

```
git add main.c
```

然后提交：

```
git commit -m "feat: complete main.c"
```

这一步完成了实验要求中的第一次代码提交。
从后面的 Git 提交历史可以看到，仓库中存在：
`feat: complete main.c`
对应的提交记录。

## 4. Git 分支实验

本部分按照实验要求，在 main 和 feature 两个分支上分别修改 main.c，并让两个分支修改同一位置，从而制造 Merge Conflict。

### 4.1 创建 feature 分支

首先创建并切换到 feature 分支：

```
git switch -c feature
```

可以使用：

```
git branch
```

检查当前分支。
此时应该可以看到：

```
* feature
  main
```

说明当前已经切换到 feature 分支。

### 4.2 在 feature 分支修改 main.c

在 feature 分支中修改 main.c。
修改完成后查看状态：

```
git status
```

然后将修改加入暂存区：

```
git add main.c
```

并提交：

```
git commit -m "feat: update greeting in feature"
```

本次实验中对应的提交为：
`640ba66 feat: update greeting in feature`

### 4.3 在 main 分支进行不同修改

完成 feature 分支的提交后，切换回 main：

```
git switch main
```

然后再次修改 main.c。
这里需要特别注意：为了让之后的 merge 一定产生冲突，需要让 main 分支和 feature 分支修改同一个文件中的同一个位置。

修改完成后进行提交：

```
git add main.c
git commit -m "feat: update greeting in main"
```

本次实验中对应的提交为：
`a38aca0 feat: update greeting in main`

这样，两个分支就分别拥有了不同的修改。

## 5. 合并分支并处理冲突

### 5.1 执行 merge

切换到 main 分支后，执行：

```
git merge feature
```

由于 main 和 feature 两个分支都修改了 main.c 中的同一个位置，因此 Git 无法自动判断应该保留哪一个版本。
最终出现了 Merge Conflict。

### 5.2 查看冲突内容

发生冲突之后，Git 会在文件中加入特殊的冲突标记。
本次实验中的冲突形式如下：

```
<<<<<<< HEAD
    printf("Hello from MAIN!\n");
=======
    printf("Hello from FEATURE!\n");
>>>>>>> feature
```

这些标记分别表示：
`<<<<<<< HEAD`
表示当前 main 分支的修改。
`=======`
表示两个版本的分界线。
`>>>>>>> feature`
表示来自 feature 分支的修改。

实际操作时，VS Code 也会直接提供：
采用当前更改
采用传入的更改
保留双方更改
比较更改
等选项。

图 1 合并 feature 分支时产生的 Merge Conflict

从截图可以看到，main 分支和 feature 分支对同一行代码进行了不同修改，因此 Git 无法自动完成合并。

### 5.3 解决冲突

发生冲突后，需要根据实际需求决定最终保留的代码。
本次实验中，我在 VS Code 中检查两个分支的修改内容，然后手动确定最终版本，并删除 Git 自动添加的：
`<<<<<<< HEAD` `=======` `>>>>>>> feature`
等冲突标记。

解决完成之后，再次检查 main.c，确保代码中已经不存在冲突标记。

### 5.4 提交冲突解决结果

确认冲突已经解决后，执行：

```
git add main.c
```

然后提交：

```
git commit -m "merge: resolve conflict between main and feature"
```

本次实验中对应的 merge commit 为：
`81ebb50 merge: resolve conflict between main and feature`

这说明两个分支的修改已经成功合并。

## 6. Git 提交历史

为了检查本次实验的分支和合并过程，使用：

```
git log --oneline --graph --all
```

查看完整的 Git 提交历史。

本次实验的关键提交包括：

```
55d0d17 docs: add lab0 report
81ebb50 merge: resolve conflict between main and feature
|\
| * 640ba66 feat: update greeting in feature
* | a38aca0 feat: update greeting in main
|/
* ca67390 feat: complete main.c
```

可以看到：
main 分支产生了 a38aca0；
feature 分支产生了 640ba66；
两个分支出现分叉；
81ebb50 将两个分支重新合并；
最后在 main 分支提交实验报告 55d0d17。

图 2 Git 分支、提交和 merge 历史

从提交图可以直观看到 main 和 feature 两个分支发生了分叉，之后通过 merge commit 81ebb50 重新合并。

图 3 VS Code 图形化 Git 历史视图

## 7. 网页阅读总结

本次实验按照要求阅读了以下两篇材料：
Commit Message 规范；
语义化版本（Semantic Versioning）。

### 7.1 Commit Message 规范

通过阅读关于 Git Commit Message 的文章，我进一步理解了 Git 中 “提交” 不仅仅是保存代码当前状态，更重要的是要通过提交信息说明这一次提交具体做了什么。

在多人协作开发中，一个项目通常会产生大量提交。如果提交信息只是简单写成：
`update` `modify` `test` `fix`
那么过一段时间之后，很难从提交历史中快速判断每一次修改的目的。

比较规范的提交信息通常需要做到简洁、明确，并能够反映本次修改的主要内容。例如：

- `feat: add user login`
- `fix: solve linked list traversal bug`
- `docs: update README`

其中，feat、fix、docs 等可以帮助开发者快速判断提交的类型，而后面的文字则说明具体进行了什么修改。

在本次实验中，我也实际使用了类似的提交方式，例如：

- `feat: complete main.c`
- `feat: update greeting in feature`
- `feat: update greeting in main`
- `merge: resolve conflict between main and feature`
- `docs: add lab0 report`

通过实际操作，我发现规范的 Commit Message 对查看 Git 历史非常有帮助。
例如执行：

```
git log --oneline
```

就可以快速了解项目经历了哪些主要修改，而不需要重新查看每一次代码差异。

另外，一次提交最好对应一个相对完整、独立的修改目的。如果把很多互不相关的修改全部放在一个 commit 中，那么以后出现问题时就不容易定位，也不方便回退。

例如：

```
feat: add login
fix: solve login bug
docs: update README
```

分别记录不同目的的修改，比全部写成一个：
`update project`
更加清晰。

因此，我认为 Commit Message 实际上也是项目开发过程中的一种 “修改日志”。规范的提交信息能够帮助开发者快速理解项目历史，也能够提高团队协作和代码维护的效率。

通过这篇文章，我也认识到 Git 并不只是一个代码备份工具，而是一个能够完整记录项目演变过程的版本管理工具。

### 7.2 语义化版本（Semantic Versioning）

第二篇阅读材料介绍了 Semantic Versioning，即语义化版本控制，简称 SemVer。
它主要解决的问题是：
当软件不断更新时，如何通过版本号让开发者和使用者理解一次更新可能带来的影响。

语义化版本通常采用：
`MAJOR.MINOR.PATCH`
即：
主版本号。次版本号。修订版本号
例如：
`1.4.2`

三个数字分别具有不同含义。
**主版本号 MAJOR**
当进行不兼容的 API 修改，使原来的程序可能无法正常使用时，通常需要增加主版本号。
例如：
`1.4.2 → 2.0.0`
这表示软件发生了比较大的变化，可能存在不兼容的修改。

**次版本号 MINOR**
当增加新的功能，但仍然保持向后兼容时，可以增加次版本号。
例如：
`1.4.2 → 1.5.0`
这通常表示软件增加了一些新的功能。

**修订版本号 PATCH**
当主要进行错误修复，并且没有增加新的功能时，可以增加修订版本号。
例如：
`1.4.2 → 1.4.3`
通常表示主要进行了 bug 修复。

因此，版本号本身实际上也携带了一定的信息。
例如：
`1.3.2 → 1.3.3`
通常可以理解为主要进行了问题修复。
而：
`1.3.2 → 1.4.0`
则意味着增加了新的功能。
如果：
`1.3.2 → 2.0.0`
则需要注意可能存在不兼容的重大变化。

### 7.3 Git Commit 与语义化版本的关系

通过阅读这篇文章，我还认识到 Git Commit 和语义化版本虽然都与 “版本” 有关，但实际上解决的是不同层面的问题。

Git Commit 主要记录开发过程中具体发生了什么修改。
例如：

```
feat: add login
fix: solve login bug
fix: solve password validation bug
```

这些属于开发过程中的具体提交记录。

而 Semantic Versioning 更关注软件对外发布时不同版本之间的关系。
例如经过若干次开发和修复之后，开发者可能决定发布一个新的版本：
`1.2.0 → 1.3.0`

因此可以把二者理解为：
Git Commit 更关注开发过程中的具体变化，而语义化版本更关注软件发布版本之间的关系。

在本次实验中，我通过：
`git log --oneline --graph --all`
查看了 Git 的具体提交历史，而 SemVer 则让我进一步认识到，当项目真正进行版本发布时，还需要使用更加规范的版本号来向用户传递软件变化的信息。

以前我认为版本号只是简单的编号，通过这篇文章之后，我认识到版本号实际上具有一定的语义，可以帮助开发者和用户判断不同版本之间的兼容性和功能变化。

## 8. 为什么要学习 Git

通过本次实验，我认为学习 Git 的意义主要体现在以下几个方面。

首先，Git 可以记录代码的历史版本。
如果直接修改文件，一旦出现错误，很难知道之前的代码是什么样子。而 Git 会保存每次 commit，因此可以通过 Git 历史查看之前的修改。

其次，Git 可以帮助管理不同功能的开发过程。
开发新功能时，可以创建独立的分支。例如本次实验中创建了：
`feature`
分支。
这样就可以在不直接影响 main 分支的情况下进行开发。

第三，Git 可以支持多人协同开发。
不同开发者可以在不同分支上进行开发，完成之后再通过 merge 或 Pull Request 等方式进行合并。如果不同开发者修改了同一个位置，Git 会明确指出冲突位置，由开发者解决。

第四，Git 可以帮助开发者形成更加规范的开发习惯。
例如本次实验中使用：
`feat: update greeting in feature`
`feat: update greeting in main`
`merge: resolve conflict between main and feature`
`docs: add lab0 report`
这样的提交信息，可以让整个开发过程更加清晰。

因此，Git 不只是一个 “保存代码” 的工具，而是一个用于管理代码变化、开发分支和协作过程的完整版本控制工具。
对于以后学习数据结构、计算机系统以及进行课程项目和科研项目开发来说，掌握 Git 也能够减少代码管理方面的问题。

## 9. 实验总结

通过本次实验，我完成了 Git 基本操作以及分支管理的学习。

本次实验主要完成了以下操作：
创建 / 使用 Git 仓库 → 修改 main.c → git add → git commit → 创建 feature 分支 → feature 分支修改并提交 → main 分支修改并提交 → git merge feature → 产生 Merge Conflict → 手动解决冲突 → git add → git commit → 完成 merge

其中，之前只是知道 git commit 可以保存代码版本，但通过这次实验，我进一步理解了 Git 分支和 merge 的实际工作过程。

尤其是在两个分支同时修改同一个位置时，Git 无法直接判断应该保留哪一个版本，因此会产生冲突。解决冲突并不是简单地让 Git 自动选择，而是需要开发者理解两个版本的修改目的，然后手动确定最终代码。

此外，通过：
`git log --oneline --graph --all`
查看整个提交历史之后，可以更加直观地看到分支的创建、分叉以及最终合并过程。

本次实验也让我认识到，规范的 commit 信息、合理的分支管理以及清晰的项目历史，对于后续进行多人协作开发都非常重要。

## 10. 建议

本次实验整体能够比较完整地覆盖 Git 的基础操作。
如果后续实验继续使用 Git，可以进一步加入 Pull Request、代码审查（Code Review）、.gitignore、远程仓库协作等内容。这样可以从个人版本管理逐步过渡到实际的软件开发协作流程。

