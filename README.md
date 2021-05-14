# 誤ってsecret key 等の情報をコミットしてしまった時の対策

## 0. リポジトリを作成しよう。

### フォークする
<img width="1435" alt="スクリーンショット 2021-05-15 1 15 19" src="https://user-images.githubusercontent.com/71377103/118302612-66e79a00-b51f-11eb-8032-2765ce6e4e33.png">


### クローンする
<img width="1435" alt="スクリーンショット 2021-05-15 1 17 14" src="https://user-images.githubusercontent.com/71377103/118302636-6d761180-b51f-11eb-93f3-8d26347bd473.png">


```
git clone <リポジトリURL>
```
## 1. 機密情報の書かれたファイルをコミットする
```
git switch -c feature/01
```
#### 機密情報の書かれたファイルを作成し、他のファイルの修正と一緒にコミット
```
echo "secret=123456789" > .env
```
```
echo "hello, world" > sample.txt
```
```
git add .
```
```
git commit -m 'commit No.1'
```
```
git push origin feature/01
```

### プルリクエスト後、mainにマージする。
<img width="1435" alt="スクリーンショット 2021-05-15 1 22 03" src="https://user-images.githubusercontent.com/71377103/118302675-76ff7980-b51f-11eb-8725-af9397a2754d.png">

<img width="1435" alt="スクリーンショット 2021-05-15 1 23 09" src="https://user-images.githubusercontent.com/71377103/118302688-7c5cc400-b51f-11eb-8b53-e2ee61596587.png">

<img width="1435" alt="スクリーンショット 2021-05-15 1 24 16" src="https://user-images.githubusercontent.com/71377103/118302723-87afef80-b51f-11eb-962e-6b46ebf06c24.png">

<img width="1435" alt="スクリーンショット 2021-05-15 1 25 23" src="https://user-images.githubusercontent.com/71377103/118302741-90a0c100-b51f-11eb-9ca0-ee2c9142a182.png">

<img width="1435" alt="スクリーンショット 2021-05-15 1 25 34" src="https://user-images.githubusercontent.com/71377103/118302753-939bb180-b51f-11eb-804e-4befb46f9d7e.png">

### ローカルに反映する

```
git switch main
```
```
git pull origin main
```
```
git branch -d feature/01
```

## 2. 現在のコミット状況の確認

### コミットしたソースが表示されることを確認しましょう。
<img width="1435" alt="スクリーンショット 2021-05-15 1 26 50" src="https://user-images.githubusercontent.com/71377103/118302773-99919280-b51f-11eb-9b83-3fbe25a93021.png">


## 3. git管理から.envファイルを削除してみましょう。
```
git switch -c feature/02
```
```
git rm .env
```
```
git commit -m 'remove .env'
```
```
git push origin feature/02
```

### プルリクエスト作成後、mainブランチにマージする

## 4. 現在のmainブランチの状況を確認してみましょう。

### git管理されているソースの確認
<img width="1435" alt="スクリーンショット 2021-05-15 1 51 30" src="https://user-images.githubusercontent.com/71377103/118304908-3fde9780-b522-11eb-8570-682ac175d27c.png">

### コミット履歴の確認
<img width="1435" alt="スクリーンショット 2021-05-15 1 54 20" src="https://user-images.githubusercontent.com/71377103/118304955-4ec54a00-b522-11eb-880a-21c7be9c8721.png">

<img width="1435" alt="スクリーンショット 2021-05-15 1 55 44" src="https://user-images.githubusercontent.com/71377103/118305110-859b6000-b522-11eb-9f77-5198bb5b8ee2.png">

<img width="1435" alt="スクリーンショット 2021-05-15 1 56 18" src="https://user-images.githubusercontent.com/71377103/118305140-8a601400-b522-11eb-819e-bb5da0c243cb.png">

**コミット履歴からは消えていないことがわかりますね！**

## 5. gitの変更履歴から.envファイルを削除してみましょう。
```
git filter-branch -f --index-filter "git rm --ignore-unmatch -f .env" HEAD
```
```
git push origin --force --all
```

## 6. コミット履歴から削除できているか見てみましょう。

<img width="1435" alt="スクリーンショット 2021-05-15 2 03 29" src="https://user-images.githubusercontent.com/71377103/118305173-91872200-b522-11eb-97b0-537422b65ba8.png">

**これで完了です！**

## 最後に

> filter-branchコマンドは変更履歴を改変する強力なコマンドです。
使用する際は注意して使いましょう！

- 実行する前にソースを別でcloneするなり、バックアップをとりましょう。
- 同じリポジトリを使う他の作業者がいる場合、今ある変更をpushしてもらった後作業を止めてもらいましょう。
- filter-branch実行後、実行者以外の方はローカルのソースを削除しもう一度git cloneしましょう。

