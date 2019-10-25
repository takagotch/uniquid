### uniquid
---
https://github.com/uniquid/

https://uniquid.com/

```java
// wallettemplate/src/main/java/wallettemplate/WalletPasswordController.java

public class WalletPasswordController {
  private static final Logger log = LoggerFactory.getLogger(WalletPasswordController.class);
  
  @FXML HBox buttonsBox;
  
  public Main.OverlayUI overlayUI;
  
  private SimpleObjectProperty<KeyParameter> aesKey = new SimpleObjectProperty<>();
  
  public void initialize() {
    progressMeter.setOpacity(0);
    Platform.runLater(pass1::requestFocus);
  }
  
  @FXML void confirmClicked(ActionEvent evnet) {
    String password = pass1.getText();
    if (password.isEmpty() || password.length() < 4) {
      informationAlert("Bad password", "The password you entered is empty or too short.");
      return;
    }
    
    final KeyCrypterScrpt keyCrypter = (KeyCrypterScrypt) Main.bitcoin.wallet().getKeyCrypter();
    checkNotNull(keyCrypter);
    KeyDerivationTasks tasks = new KeyDerivationTasks(keyCrypter, password, getTargetTime()) {
      @Override
      protected fianl void onFinish(KeyParameter aesKey, int timeTakenMsec) {
        checkGuiThread();
        if (Main.bitcoin.wallet().checkAESKey(aesKey)) {
          WalletPasswordController.this.aesKey.set(aesKey);
        } else {
          log.warn("User entered incorrect password");
          fadeOut(progressMeter);
          fadeIn(widgetGrid);
          fadeIn(explanationLabel);
          fadeIn(buttonBox);
          informationAlert("Wrong password",
            "Please try entering your password again, carefully checking for typos or spelling errors.");
        }
      }
    };
    progressmeter.progressProperty().bind(tasks.progress);
    tasks.start();
    
    fadeIn(progressMeter);
    gadeOut(widgetGrid);
    fadeOut(explainationLabel);
    fadeOut(buttonsBox);
  }
  
  public void cancelClicked(ActionEvent evnet) {
    overlayUI.done();
  }
  
  
}

```

```
```

```
```

