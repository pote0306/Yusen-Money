YusenMoney/
├── lib/
│   ├── main.dart
│   ├── screens/
│   │   ├── home_screen.dart
│   │   ├── wallet_screen.dart
│   │   └── withdraw_screen.dart
│   └── services/
│       ├── admob_service.dart
│       └── firebase_service.dart
├── assets/
│   ├── logo/ysm_logo.png
│   └── mockup/ (ภาพหน้าจอแอพ)
├── firebase/
│   └── index.js
├── pubspec.yaml
└── README.md
users/
  {userId}/
    name: "ชื่อผู้ใช้"
    points: 150
    withdraw_requests/
      {requestId}/
        amount: 50
        method: "TrueMoney"
        status: "pending"
        import 'package:flutter/material.dart';
import 'package:google_mobile_ads/google_mobile_ads.dart';

void main() {
  WidgetsFlutterBinding.ensureInitialized();
  MobileAds.instance.initialize();
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Earn Money App',
      home: HomeScreen(),
    );
  }
}

class HomeScreen extends StatelessWidget {
  void _showRewardedAd() {
    RewardedAd.load(
      adUnitId: '<YOUR_ADMOB_UNIT_ID>',
      request: AdRequest(),
      rewardedAdLoadCallback: RewardedAdLoadCallback(
        onAdLoaded: (ad) {
          ad.show(
            onUserEarnedReward: (ad, reward) {
              print('User earned: ${reward.amount}');
              // เรียก API อัปเดตแต้ม
            },
          );
        },
        onAdFailedToLoad: (error) => print(error),
      ),
    );
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Earn Money')),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            Text('แต้มของคุณ: 100'),
            SizedBox(height: 20),
            ElevatedButton(
              onPressed: _showRewardedAd,
              child: Text('ดูโฆษณารับแต้ม'),
            ),
            SizedBox(height: 20),
            ElevatedButton(
              onPressed: () {
                Navigator.push(context,
                    MaterialPageRoute(builder: (_) => WalletScreen()));
              },
              child: Text('กระเป๋าเงิน'),
            ),
          ],
        ),
      ),
    );
  }
}

class WalletScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('กระเป๋าเงิน')),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            Text('แต้มของคุณ: 100'),
            SizedBox(height: 20),
            ElevatedButton(
              onPressed: () {
                // เรียก API ถอนเงิน
              },
              child: Text('ถอนเงิน'),
            ),
          ],
        ),
      ),
    );
  }
}
