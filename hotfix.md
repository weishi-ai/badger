# windows 重启问题的热修复

* 什么问题？
  * 在windows断电后，再次自动，badgerKv panic，导致使用badgerKv的主程序也crash
  * 具体参考 [问题 1883](https://github.com/dgraph-io/badger/issues/1883)

* 如何修复
  * 参考 [fix 2033](https://github.com/dgraph-io/badger/pull/2033)

* 如何将修复cherrypick到本地
  * fork了 `https://github.com/dgraph-io/badger`，到 `https://github.com/xxx-ai/badger`
  * checrry `https://github.com/VinGarcia/badger` 的 `add-panic-handler` 分支 cherry-pick到我的fork
    * 确保你的本地仓库是最新的：

    * 确保你已经将你的fork克隆到了本地，并且你正在你想要应用cherry-pick的分支上。
    添加VinGarcia的仓库为远程仓库（如果你还没有这么做的话）：

    * 在你的本地仓库中，添加VinGarcia的仓库为一个新的远程仓库，你可以将其命名为vingarcia。
     `git remote add vingarcia https://github.com/VinGarcia/badger.git`

    * 从VinGarcia的仓库拉取特定分支的最新更改：

        使用git fetch命令拉取add-panic-handler分支的最新更改。
        `git fetch vingarcia add-panic-handler`
    * 执行cherry-pick操作：
        使用git cherry-pick命令加上你想要cherry-pick的commit ID。
        `git cherry-pick 548c3cee3e9cae4946e90418f6160136c34ee29a`

    * 如果cherry-pick操作产生了冲突，你需要手动解决这些冲突，并使用git cherry-pick --continue完成cherry-pick操作。
    将更改推送到你的fork：

    * 一旦cherry-pick完成并且所有冲突都解决了，你就可以将更改推送到你的fork上了。
        `git push origin <你的分支名>`
        请替换<你的分支名>为你当前工作的分支名。这样，你就成功地将特定的commit从VinGarcia的add-panic-handler分支cherry-pick到了你的fork。

    ```
* 如何使用fork的仓库作为module添加到程序中
  如果您需要立即使用您的修复版本，可以在您的Go项目中使用replace指令来指定您Fork的版本。在您的go.mod文件中添加如下行：
  
  `·replace module_name => github.com/your_username/repository_name@branch_name`

  这样，当您运行go build或go mod tidy时，Go工具链会使用您指定的Fork和分支上的代码，而不是原始模块的代码。

  记得在原始仓库的Pull Request被接受并合并后，更新您的go.mod文件以使用官方版本，同时移除replace指令。