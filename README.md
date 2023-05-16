# Ionic Capacitor Standalone AppLovin (Cordova) Integration
Integrate AppLovin Cordova plugin with Ionic Capacitor Application

1. Follow [these steps](https://dash.applovin.com/documentation/mediation/cordova/getting-started/integration) to install the Cordova plugin in your Ionic Capacitor Application
    - Instead of running `cordova plugin add cordova-plugin-applovin-max` to install the plugin
    - Run `npm i cordova-plugin-applovin-max --save`, check [this tutorial](https://capacitorjs.com/docs/v2/cordova/using-cordova-plugins) for more details.
    - Run `npx cap sync`

2. In your Standalone component
    - Declare a **local variable** inside your **component class** `AppLovinMAX: any;`
    - **Initialize** the variable inside the **constructor** `this.AppLovinMAX = (<any>(window).window).applovin`

3. In your `ionViewDidEnter` function
    - Initialize the AppLovin SDK `this.AppLovinMAX.initialize('YOUR_ACCOUNT_API');`
    - Create a banner `this.AppLovinMAX.createBanner('BANNER_ID', this.AppLovinMAX.AdViewPosition.TOP_CENTER);`
    - Show the banner `this.AppLovinMAX.showBanner('BANNER_ID');`

4. That is it, there is no step 4, you're done!
    - Make sure you add your device to the test devices from your AppLovin Dashboard

## Complete Code Sample
```TypeScript
import { CommonModule } from '@angular/common';
import { IonicModule, Platform } from '@ionic/angular';
import { Component } from '@angular/core';

@Component({
  selector: 'app-tab1',
  templateUrl: 'tab1.page.html',
  styleUrls: ['tab1.page.scss'],
  standalone: true,
  imports: [IonicModule, CommonModule],
})

export class Tab1Page {
  AppLovinMAX: any;
  
   constructor(private platform: Platform) {
    this.AppLovinMAX = (<any>(window).window).applovin
  }
  
  async ionViewDidEnter() {
    await this.initializeAppLovin()
  }
  
  async initializeAppLovin() {
    const isMobile = this.platform.is('mobile');
    const canAppLovin = this.AppLovinMAX !== 'undefined';
    if (isMobile && canAppLovin) {
      this.AppLovinMAX.setVerboseLogging(true);
      this.AppLovinMAX.initialize('YOUR_ACCOUNT_API');
      this.AppLovinMAX.createBanner(
        'BANNER_ID',
        this.AppLovinMAX.AdViewPosition.TOP_CENTER);
        this.AppLovinMAX.showBanner('BANNER_ID');
    }
  }
}
```
