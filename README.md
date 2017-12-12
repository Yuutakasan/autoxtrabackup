autoxtrabackup
==============

Percona innobackupex(xtrabackup) を使用した自動MySQLスケジュールバックアップ。
このスクリプトでは、percona-xtrabackupに含まれるPerconaのxtrabackup用innobackupexラッパーを使用しています。

構成可能な保存と圧縮、およびオプションの電子メール出力を使用して、完全バックアップと増分バックアップを自動的に作成します。

Requirements
------------
 - サポートされているMySQLディストリビューション:MySQL、Percona Server、MariaDB
 - サポートされているLinuxディストリビューション:Debian、Ubuntu、CentOS、RedHat
 - 依存関係：percona-xtrabackup、からのダウンロード http://www.percona.com/software/percona-xtrabackup

このスクリプトはPercona-server 5.6のDebian 7（Wheezy）でテストされています。
このスクリプトはCentOS 6.4でMariaDB-server-10でテストされています。

Installation
------------
autoxtrabackup.configを　/etc/default/autoxtrabackup　にコピーし、設定を編集してください。
これは必須ではありませんが、推奨です。また、スクリプト内の設定を直接変更することも可能です。
autoxtrabackup.shを/usr/local/bin/autoxtrabackupにコピーします。
実行可能にし、cronjobを設定する

スクリプトは標準出力を提供しません。 /tmp/backuplog

Examples
---------
1時間ごとに増分バックアップを作成し、24時間ごとに完全バックアップを作成します。1週間バックアップを保持する。
  - Set "hoursBeforeFull" to 24  
  - Set "keepDays" to 7  
  - Add a cronjob "0 * * * * /usr/local/bin/autoxtrabackup"

日曜日に完全バックアップを作成し、他のすべての日に増分バックアップを作成します。バックアップを1ヶ月間保持する
  - Set "hoursBeforeFull" to 168
  - Set "keepDays" to 31
  - Create the first backup on Sunday at the desired time, let's take 23h for example
  - Add a cronjob "0 23 * * * /usr/local/bin/autoxtrabackup"

増分バックアップを作成せず、毎日23時にフルバックアップを作成し、1週間バックアップを保持する
  - Set "hoursBeforeFull" to 1
  - Set "keepDays" to 7
  - Add a cronjob "0 23 * * * /usr/local/bin/autoxtrabackup"

Remarks
-------
インクリメンタルバックアップは、XtraDB＆InnoDBテーブルにのみ適用されます。
MyISAMテーブルの増分バックアップは不可能です。毎回、そのようなテーブルのフルバックアップが作成されます。

Restoring
---------
バックアップを復元する方法については、「./autoxtrabackup -h」を参照してください。

innobackupexでバックアップを復元する方法の詳細については、次のサイトを参照してください。 http://www.percona.com/doc/percona-xtrabackup/2.2/innobackupex/innobackupex_script.html
