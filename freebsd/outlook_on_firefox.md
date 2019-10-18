# FirefoxでWeb版Outlook Mail Calenderにアクセスすると軽量モードしか使えない問題を解決する。

この問題の症状は [Firefox looks different with outlook.com mail](https://forums.freebsd.org/threads/firefox-looks-different-with-outlook-com-mail.69066/) の通り。Chromiumでは問題ないが、Firefoxでアクセスすると強制的に軽量モードのOutlookしか使えなくなってしまう。UI/UXはリッチなOutlookのほうが優れているのでこれは不便。

このフォーラムにある通り、ブラウザのUserAgentを偽装してやることで問題は解決する。

1. URL欄に `about:config` と入力しEnter
2. 文字列型の設定値を新規作成する（右クリックする）。名前は `general.useragent.override` で、値はたとえば `Mozilla/5.0 (X11; Linux x86_64; rv:69.0) Gecko/20100101 Firefox/69.0 ` のようにLinuxであるかのように偽装すればいい。値が適当だとOutlook以外の、UserAgentを見てなにかしらの処理をおこなうJavaScriptが動くページで弾かれる可能性がある。
3. Outlookに再びアクセスする。
