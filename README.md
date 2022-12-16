itaichi ナレッジまとめ
◯Rails
 アドレスの階層を変えたい
 　例えば、cardsをapi/v1/pokerにしたかったら...
 　code:cards_controller
 　 CardsController→Api::V1::PokerController
 　app/controllers下にapiというフォルダを作成し、その下にv1というフォルダを作成、その下にcards_controllerを配置
 　app/viewでも同じ
 レスポンスステータス
 　railsはwebサーバーのプログラムなので、プログラムの中にレスポンスステータスを返すコードを用意する必要がある
 　[https://netprotections.gyazo.com/d8516008b84b84584549f6ef0867b328]
　pry-byebug
　 gemfileにgem 'pry-byebug'、gem 'pry-rails'が入っていれば使える
　 変数を確認したい箇所にbinding.pryとかくだけ
　 rspecでも使える
　bundle install
　 NPではフォルダごとにrubyのバージョンを管理しているので、bundle install --path vendor/bundleとする
　 バージョンは1.17.3。Gemfile.lockの一番下の行に1.17.3とあればOK
　APIモードとはhttps://techtechmedia.com/api-mode-rails/
　　viewがない
　　通常のRailsアプリケーションではerbファイルをレスポンスとして返すが、APIモードではJSONをレスポンスに返す
　railsでモデル作成時、idとcreated_atとupdated_atは自動で生成され、再定義不可
　アドレスの?以降がクエリーパラメータ
　　?id=1&amount=1000のように書くhttps://www.javadrive.jp/rails/controller/index6.html#section2
　 簡単で便利
　 クエリーパラメータだけならリクエスト形式はGET
　モデルやコントローラーを間違えて作っちゃったら
　　rails destroyhttps://yuu.1000quu.com/delete_the_model_and_controller_in_rails
　　できなかったらbin/rails destroyを試してみる
　一つのカラムを変更できるメソッド
　　変数.update_attribute(:カラム名, 変更後の値)
　テスト環境でrails c
　 code:テスト環境のrails c
   rails c -e test
   Rails.env //確認
 新しいレポジトリを触るとき
 　code:ブランチ作成
 　 git checkout develop
   git pull origin develop
   git checkout -b feature-ATN-〇〇〇〇
  code:database.yml作成
   cd config
   cp database.yml.example database.yml
   cd ..
   // database.ymlを開いてatonedb_rapi_testからatonedb_testに書き換え
 　code:機密情報の復号化
  　　bundle exec rake yaml_vault:decrypt"[settings.yml,settings/development.yml,settings/test.yml]"
 リモートで本番の動作確認をしたい
 　https://np-develop.backlog.jp/alias/wiki/240548
 NPTLにローカルでログインしたい
 　[atone開発 作業ログ 上村 NPTLをローカルで見られるようにする]
 SPTLにローカルでログインしたい
 　code:shop_userのパスワード変更
 　 cd paleale-sptl
   rails c
   > shop_user1 = ShopUser.find(1)
   > shop_user1.password = 'test1234'
   > shop_user1.save!
   > exit
  code:app/models/shop_user.rb
   def switchable?(access_id)
     true
     # selected_inclusive_shops(access_id).present? || selected_shops(access_id).present?
   end
  rails s→企業コード：atn00001, ログインID：fufururu, pass：test1234でログインできる
  表示されない箇所は、viewファイルの該当箇所のif文をtrueにすることで表示できる
	before_action
	 skip_before_actionで書けば、継承元のcontrollerで設定したbefore_actionを無視することができる
	routes.rb
		resources
 	 resourcesの中にresourcesを書くとcontroller1/id/controller2みたいなルーティングになる
 	 https://pikawaka.com/rails/resources
 	 collection doは間にid挟まなくて良くなる
 	 https://qiita.com/k152744/items/141345e34fc0095217fe
 　routes.rbの行末のasはPrefixed_pathとPrefixed_urlを生成するもの
 　　Prefix_path：https://qiita.com/fuku_tech/items/8be0a2a889839ca47ae0
  　https://qiita.com/xusaku_/items/c5c9137d580db1c19a22
 viewファイルのコメントアウト
 　<% if false %>〇〇<% end %>
 　https://qiita.com/Masashi9410/items/16ce1c6da64eae497615
 エラー発生時にrails 標準のエラーが出てしまう場合
 　code:application_controller
   # 例外ハンドル
   unless Rails.env.development?
     rescue_from Exception, with: :rescue500
     rescue_from ActionController::RoutingError, with: :rescue404
     rescue_from ActionController::InvalidAuthenticityToken, with: :invalid_authenticity_token
   end
  これのunlessをifに変える
 find, find_by, whereの違い
 　https://qiita.com/nakayuu07/items/3d5e2f8784b6f18186f2

◯VSCode
 縦選択
 　開始位置クリック→終了位置をoption + shift + クリック
 　command + option + ↓ で複数行の頭に挿入
 　fn + → で行末に移動
 ショートカット
 　開いているファイル内で検索：command + f
 　全ファイルで検索：command + shift + f
 　ファイル名で検索：command + p
 　戻す：command + z
 　やっぱり戻さない：command + shift + z
　　全部保存：option + command + S
　　全部閉じる：command + K → U

◯finder
 パスを指定して飛ぶ：command + shift + g
 非表示ファイルを表示：command + shift + .
 フルパスコピー：ファイル選択してoption + command + C

◯ターミナル(linuxコマンド)
	ターミナル上でテキストファイルに書き込み：vim
 	挿入：i、編集：R、モードの終了：esc
 	保存して終了：:wq

◯postgresql
 ダンプファイルリストア
　　[atone開発環境構築　DBダンプファイル取得について]←atone-dev-roleがあればここから.dumpをダウンロード
　　1. database.ymlの設定
　　atoneのコードにはdatabase.example.ymlしかないので案内に従ってdatabase.ymlを作成
　　test環境の参照するデータベースを変更(例：atonedb-nptl-test→atonedb-test)
　　2. データベースの作成
　　まず起動https://qiita.com/domodomodomo/items/12fe7555513de6b078db ：brew services start postgresql
　　ユーザー"i04npdb"を作成、権限を付与：https://np-develop.backlog.jp/wiki/ATN/環境%2F開発環境%2Fエンジニア開発環境の構築%EF%BC%88Mac版%EF%BC%89
　　ログインhttps://zenn.dev/nevil/articles/7a606da871f1c412a86d ：psql -d postgres -U i04npdb
　　CREATE DATABASE atonedb_dev;
　　\q
　　3. 準備
　　そのままやるとschema "public" already existsが出るのでスキーマの削除
　　psql -d atonedb_dev -U i04npdb
　　\dnを実行するとpalealeとpublicがあると出るので二つをDROP SCHEMA https://www.flyenginer.com/low/low_db/postgresql_low/postgresqlスキーマとテーブルに関するコマンド.html#toc2
　　4. dumpファイルをダウンロード
　　[ここ https://s3.console.aws.amazon.com/s3/buckets/prod-atone?region=ap-northeast-1&prefix=db_dump/]からダウンロード
　　5. リストア
　　cd downloads
　　pg_restore -e --dbname=atonedb_dev atonedb_dev_20210511.dump
　　 https://zatoima.github.io/postgresql-about-pg_dump-pg_restore.html
　　6. 足りないテーブルをマイグレーション
  bundle exec rake db:migrate
 　途中で引っかかる→特定のマイグレーションファイルをスキップ
  　posticoでctrl + P→schema_migrations
  　+rowボタンを押して、一番下にスキップしたいファイルの頭の数字(例：20211027100828)を入力→save
  　もう一回bundle exec rake db:migrate
 psqlをデータベースにログインせず使う
  psql データベース名 -c "psqlコマンド"
　特定のカラムを持つデータベース一覧
　　select table_name, column_name from information_schema.columns where column_name ='検索するカラム名';

◯Postman
 1. リクエスト形式を選択(GET,POST)
 2. URLを入力
 3. Body→raw→JSONを選択
 4. JSON形式(ハッシュみたいなやつ)でデータを入力

◯mac全般
　戻す：command + z
 やっぱり戻さない：command + shift + z
 フルスクリーン状態でウィンドウの切り替え：３本指でスワイプ
　スリープしないように設定変更
　　https://yama-mac.com/prevent_sleep_when_lid_close/#:~:text=▶%20メニューが表示され,のチェックを外します%E3%80%82
　 システム環境設定→バッテリー

◯git
 code:リモートの親ブランチの変更をリモートの子ブランチに反映
  git checkout develop
  git pull
  git checkout feature-ATN-◯◯◯
  git merge develop
  conflictが出るので、変更前のものを一時別場所に避難
  git add .
  git push
  避難させたものをコピペしてまたgit add .、 git push
 コマンドラインでのgit
  git add -n .：ステージ可能なファイル一覧
  git add ファイル名：ステージ、ファイル名→.で全部のファイル
  git commit -m 'コミットメッセージ'：コミット
  git push origin：リモートにプッシュ(変更の同期)
   git push --set-upstream origin 'ブランチ名'：リモートにブランチを作成、プッシュ
  git checkout .：変更の破棄
  git clean -f：作成したファイルの破棄
 タグ
  タグはコミット単位でつけるもの
  　バージョンって考えればいい
  code:タグ上げ
  　git tag "feature-ATN-XXXXX-0.1.0"
   git push origin "feature-ATN-XXXXX-0.1.0"
  gemfileのtagの箇所を修正すれば参照できる
 別ブランチでやりたい作業があるとき
 　code:stash
 　 git stash -u
 　 git stash list // stashした変更一覧
 　 git stash apply stash@{n} //　復元
 間違えてコミットした時
 　code:取り消し
 　 git reset --soft HEAD^
 　　// コミットしたくない変更のステージングを解除
 　　git push -f

◯Grape
 https://qiita.com/ftyabu/items/0e97394f2263171cf469
　 APIにおけるMVC
  Model：いつも通り
  View：返す情報の定義、rablを使って記述される
  Controller：いつも通り
  　paramsメソッド：取得したい情報を記述できる
　code:paramsの使い方
　 params do
 　  requires :値, type: データ型　　　　　// 必ず取得する
  　　 optional :値, type: データ型　　　　　// なくても回る
 　end 
　helpersメソッド：中でメソッドを定義すると同じファイル内でメソッドが使えるようになる
　descメソッド：それ以降の処理の説明、コメントアウトみたいなもん
　get,post,put,patch：リソースの取得、新情報の登録、リソースの上書き、リソースの一部変更
　resource :名前 do：アクセスするapiのアドレスを指定する
　expose：APIでレスポンスに含めたい属性を指定する
　describe：どのファイル、メソッドを対象にしているのかの明示、コメントアウトみたいなもん

◯Docker
　macやwindowsでlinux環境を使いたいときに便利
　https://qiita.com/gahoh/items/92217e0a887bb81e3155

◯rspec
 テスト用のデータベースの用意
 　dumpファイルで作ったのはdevelop(開発)環境
 　今から作るのはrspec用のデータベース
 　code:test環境
 　　psql -d postgres -U i04npdb;
 　　CREATE DATABASE atonedb_test;
 　　\q
 　　bundle exec rake db:migrate RAILS_ENV=test
 　rspecが動かないとき→`bundle exec rake db:migrate:reset RAILS_ENV=test`
 　 注意) RAILS_ENV=testをつけ忘れるとdumpファイルのリストアをやり直すことになる
 実行
 　code:rspec
 　 bundle install --path vendor/bundle
   RAILS_ENV=test bundle exec rake db:migrate:reset
 　 bundle exec rspec
 common改修
 　ローカルの他のレポジトリからローカルのcommonを参照してrspec回したい
 　code:Gemfile
 　 gem 'paleale_common', path: '../paleale-common'
 　code:ターミナル
 　 cd config
 　 cp database.yml.example database.yml
 　 cd ..
 　code:database.yml
 　 test:
     <<: *default
     database: atonedb_test
     username: i04npdb
     password: i04npdb
  code:最新化
  　git checkout develop
  　git pull origin develop
  　git checkout feature-ATN-15820-2
  　git merge develop
  　RAILS_ENV=test bundle exec rake db:migrate:reset //改めてマイグレーション
  code:ターミナル
 　　bundle install --path vendor/bundle
 　　bundle exec rspec
 途中で止めた時
 　code:ターミナル
 　 bundle exec rake db:drop db:create RAILS_ENV="test"
   bundle exec rake db:migrate RAILS_ENV="test"
 エラーログ
 　たとえば、`MABT-BS-BL-0049`のバッチのrspecを回した時のログは`log/BATCH_MAC0325_MABT-BS-BL-0049_batchscript_trace.log`

◯Factory Girl
 テスト用のサンプルデータを作ってくれる
 　rspecのspecファイルの中の`create(:'データベース名', ... )`でサンプルデータが作成される
 atoneではcommonで管理されてる
 　作成されるデータの中身が定義されている
 rspec実行時でなくても、rails cから使える
  code: factory girl
   bundle install --path vendor/bundle //事前にgemfileのgem factory_girl_railsをtestの外に出しておいた
   rails c --sandbox //これをやるとquitした時に作成したデータは破棄される
   include CodeDefinition　//以降console上
   include ClassificationDefinition
   require 'factory_girl_rails'
   include FactoryGirl::Syntax::Methods
   FactoryGirl.definition_file_paths << PalealeCommon::Engine.root.join('spec', 'factories')
   FactoryGirl.reload
  これをすると、例えば`FactoryGirl.create(:member)`でデータが作成される
  　読み込まれるファイルはpaleale-common/spec/factories/members.rb

◯review
 binding.pryをrubyで使う
 　ターミナルで`gem install pry-byebug`
 　ファイルの頭に`require 'pry'`
 railsの時はGemfileに`gem 'pry-byebug'`
