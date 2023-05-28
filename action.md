# Action
下面是一个使用 GitHub Actions 监听 Issue 评论创建事件，并在评论属于指定用户时为该评论添加互动 reaction（例如 "eyes"）的示例：

```yaml
Copy code
name: Add Reaction to Comment
on:
  issue_comment:
    types:
      - created

jobs:
  add-reaction:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout repository
        uses: actions/checkout@v2

      - name: Add reaction
        uses: octokit/request-action@v2.x
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
        with:
          route: POST /repos/${{ github.repository }}/issues/comments/${{ github.event.comment.id }}/reactions
          media-type: '{"reactions": "eyes"}'
          headers: '{"Accept": "application/vnd.github.squirrel-girl-preview+json"}'
          if: contains(['user1', 'user2', 'user3'], github.event.comment.user.login)
```
上述示例中，当有新的 Issue 评论创建时，GitHub Actions 将运行名为 "add-reaction" 的 job。该 job 在 Ubuntu 环境下运行。首先，它会检出仓库。然后，它使用 octokit/request-action 操作来向指定的评论添加 "eyes" reaction。要注意的是，此操作需要设置 GITHUB_TOKEN 环境变量来进行身份验证，并在请求头中添加 Accept 标头以指定使用 Squirrel Girl 预览版本的 GitHub API。

最后，通过使用 if 条件，我们可以检查评论的作者是否属于指定的用户列表。如果是，则执行添加 reaction 的步骤；否则，跳过该步骤。

请根据实际情况将示例中的 'user1', 'user2', 'user3' 替换为您希望添加 reaction 的用户列表。
