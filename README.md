n# Wifi-vocer
Wifi vocer 
import 'package:flutter/material.dart';
import 'pages/login_page.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'WiFi Voucher Admin',
      debugShowCheckedModeBanner: false,
      home: LoginPage(),
    );
  }
}

import 'package:flutter/material.dart';
import 'dashboard_page.dart';

class LoginPage extends StatelessWidget {
  final userCtrl = TextEditingController();
  final passCtrl = TextEditingController();

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Padding(
        padding: const EdgeInsets.all(20),
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            Text('ADMIN LOGIN', style: TextStyle(fontSize: 24)),
            TextField(controller: userCtrl, decoration: InputDecoration(labelText: 'Username')),
            TextField(controller: passCtrl, decoration: InputDecoration(labelText: 'Password'), obscureText: true),
            ElevatedButton(
              child: Text('LOGIN'),
              onPressed: () {
                Navigator.push(context,
                  MaterialPageRoute(builder: (_) => DashboardPage()));
              },
            )
          ],
        ),
      ),
    );
  }
}
import 'package:flutter/material.dart';
import 'generate_page.dart';

class DashboardPage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Dashboard Admin')),
      body: Center(
        child: ElevatedButton(
          child: Text('Generate Voucher'),
          onPressed: () {
            Navigator.push(context,
              MaterialPageRoute(builder: (_) => GeneratePage()));
          },
        ),
      ),
    );
  }
}
import 'package:flutter/material.dart';
import '../services/api_service.dart';

class GeneratePage extends StatefulWidget {
  @override
  _GeneratePageState createState() => _GeneratePageState();
}

class _GeneratePageState extends State<GeneratePage> {
  String profile = '1jam';
  String result = '';

  void generate() async {
    final voucher = await ApiService.generateVoucher(profile);
    setState(() {
      result = voucher;
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Generate Voucher')),
      body: Padding(
        padding: const EdgeInsets.all(20),
        child: Column(
          children: [
            DropdownButton(
              value: profile,
              items: ['1jam', '1hari']
                  .map((e) => DropdownMenuItem(value: e, child: Text(e)))
                  .toList(),
              onChanged: (v) => setState(() => profile = v!),
            ),
            ElevatedButton(
              onPressed: generate,
              child: Text('GENERATE'),
            ),
            SelectableText(result, style: TextStyle(fontSize: 20))
          ],
        ),
      ),
    );
  }
}
