# VSCode ile ilgili ayarlar
Başlangıç Tarihi 09/04/2021 15:08   
rev.2. 13/8/21   
Yazan: Cengizhan Parlak [GitHub](https://github.com/cengizhanparlak)
<hr>

## *İçerik* <a id=icerik></a>

## 1) [Araçlar, IDE Ayarları, Eklentiler, Emmets](#notlar-araclar)

  * [Yararlı VS Code kısayolları](#notlar-kisayollar)
  * [IDE Ayarları](#notlar-ide-ayarlar)

# Araçlar, IDE Ayarları, Eklentiler <a id="notlar-araclar"></a> [&#8593;](#icerik) 

Kod yazımında kullanılan ek araçlar, kullanımları üzerine notlar. IDE Ayarları ve eklentiler; nasıl kullanılması gerektiği ve özellikleri hakkında notlar

## Yararlı VS Code kısayolları <a id="notlar-kisayollar"/></a> 
* `Ctrl + enter` aşağıya bir boş satır ekler
* `Ctrl + Shift + enter` yukarıya bir boş satır ekler
* `Ctrl + Shift + E` dosya panelini açar
* `Ctrl + Shift + G` source control panelini açar
* `Ctrl + Shift + X` eklenti marketini açar
* `Ctrl + P` Workspace içinde dosya araması
* `Ctrl + Shift + P` VS Code ayarlarında arama (alternatif olarak `>` işaretiyle çalışır)
* `Ctrl + P` workspace içindeki dosya ve klasörleri aramanızı sağlar
* `Ctrl + Shift + N` yeni bir VS Code penceresi açar
* `Ctrl + Shift + "` dahili terminali açar
* `Ctrl + Shift + C` harici terminali (windows cmd) açar
* `Ctrl + B`  sidebar'ı kapatıp açabilirsiniz
* `Ctrl + J`  terminal panelini açma/kapama
* `Ctrl + K V` kısayolu ile sağ kısımda yeni ön izleme açılabilir
* `Ctrl + Shift + V` kısayolu ile ön izleme aynı tab grubunda açılır
* `Ctrl + Shift + K` satır silinir
* `Ctrl + D` Seçilen text'in sonraki bulunduğu yere de cursor atar
* `Alt + Shift + F` ile kod düzenlemesi yapılabilir.
* `Ctrl + K + C` ile satır yorum satırı haline getirilir.
* `Ctrl + K + U` ile satırdaki 1 yorum kaldırılır.
* `Ctrl + M P` matching paire gider.
* `Ctrl + Space` veya `Ctrl + .` ile IntelliSense'in olduğu herhangi bir editörde o satır için önerilenler kısmını tekrar açabilirsiniz.
* `Ctrl + 1/2/3` tab grupları arasında gezinti yapmayı sağlar
* `Ctrl + Alt + sağ/sol ok` açık olan tabı sağ gruba taşır. sağda açık grup yoksa ekranı ikiye böler ve yeni grup açar
* `Alt + LClick` seçilen sayıda satırdaki yeri aynı anda değiştirmek için kullanılır. (Multiline editing)
## IDE AYARLARI <a id="notlar-ide-ayarlar"></a>

* `Ctrl + N` ile bulunduğunuz klasörde hızlıca dosya açmak için şu adımlar yapılmalı
  1. VS Code ayarlarında (`Ctrl + Shift + P`) şu dosyayı aratıp açın 
  
         Preferences: Open Keyboard Shortcuts (JSON)
  2. Aşağıdaki kodu yapıştırıp dosyayı kaydedin
        
          [
            { 
              "key": "ctrl+n", 
              "command": "explorer.  newFile" },
           ]
           
* `Ctrl + Alt + N` ile workspace'de hızlıca klasör yaratmak için yukarıdaki adımı takip edin. 2. adımda süslü parantezden virgül koyup şu kodu ekleyin 

      { 
        "key": "ctrl+alt+n", 
        "command": "explorer.newFolder" },
  
  Her ikisini de eklediyseniz kod şöyle olmalı 
  
      [
          { 
          "key": "ctrl+n", "command": "explorer.newFile" }, 
          { 
          "key": "ctrl+alt+n", "command": "explorer.    newFolder" },
      ]

*   `Ctrl + Tab` ve `Ctrl + Shift + Tab` ile sırayla sekme gezintisi yapmak için `settings.json` içine ekleyin 

        {
            "key": "ctrl+tab", 
            "command": "workbench.action.nextEditorInGroup" },
        {
            "key": "ctrl+shift+tab",
            "command": "workbench.action.previousEditorInGroup" },