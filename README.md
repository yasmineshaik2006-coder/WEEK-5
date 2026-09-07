week-5(a)
import 'package:flutter/material.dart';

void main() {
  runApp(const MyToggleApp());
}

class MyToggleApp extends StatelessWidget {
  const MyToggleApp({super.key});

  @override
  Widget build(BuildContext context) {
    return const MaterialApp(
      debugShowCheckedModeBanner: false,
      home: ToggleScreen(),
    );
  }
}

class ToggleScreen extends StatefulWidget {
  const ToggleScreen({super.key});

  @override
  State<ToggleScreen> createState() => _ToggleScreenState();
}

class _ToggleScreenState extends State<ToggleScreen> {
  bool isOn = false;

  void toggleSwitch(bool value) {
    setState(() {
      isOn = value;
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Toggle Switch'),
        backgroundColor: Colors.teal,
        foregroundColor: Colors.white,
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            Switch(
              value: isOn,
              onChanged: toggleSwitch,
            ),
            const SizedBox(height: 10),
            Text(
              isOn ? 'Switch is ON' : 'Switch is OFF',
              style: const TextStyle(
                fontSize: 22,
                fontWeight: FontWeight.bold,
              ),
            ),
          ],
        ),
      ),
    );
  }
}



week-5(b)
import 'package:flutter/material.dart';
import 'package:provider/provider.dart';

// Provider class
class CounterProvider with ChangeNotifier {
  int _count = 0;

  int get count => _count;

  void increment() {
    _count++;
    notifyListeners();
  }
}

void main() {
  runApp(
    ChangeNotifierProvider(
      create: (_) => CounterProvider(),
      child: const MyApp(),
    ),
  );
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return const MaterialApp(
      debugShowCheckedModeBanner: false,
      home: HomeScreen(),
    );
  }
}

class HomeScreen extends StatelessWidget {
  const HomeScreen({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('setState vs Provider'),
        backgroundColor: Colors.red,
      ),
      backgroundColor: Colors.lightBlue,
      body: const Column(
        children: [
          Expanded(
            child: SetStateCounter(),
          ),
          Divider(
            thickness: 2,
          ),
          Expanded(
            child: ProviderCounter(),
          ),
        ],
      ),
    );
  }
}

// ---------------- SETSTATE COUNTER ----------------

class SetStateCounter extends StatefulWidget {
  const SetStateCounter({super.key});

  @override
  State<SetStateCounter> createState() => _SetStateCounterState();
}

class _SetStateCounterState extends State<SetStateCounter> {
  int _count = 0;

  void _increment() {
    setState(() {
      _count++;
    });
  }

  @override
  Widget build(BuildContext context) {
    return Center(
      child: Column(
        mainAxisAlignment: MainAxisAlignment.center,
        children: [
          const Text(
            'setState Counter',
            style: TextStyle(
              fontSize: 30,
              fontWeight: FontWeight.bold,
            ),
          ),

          const SizedBox(height: 10),

          Text(
            '$_count',
            style: const TextStyle(
              fontSize: 56,
            ),
          ),

          ElevatedButton(
            onPressed: _increment,
            child: const Text('Increment'),
          ),
        ],
      ),
    );
  }
}

// ---------------- PROVIDER COUNTER ----------------

class ProviderCounter extends StatelessWidget {
  const ProviderCounter({super.key});

  @override
  Widget build(BuildContext context) {
    final counter = context.watch<CounterProvider>();

    return Center(
      child: Column(
        mainAxisAlignment: MainAxisAlignment.center,
        children: [
          const Text(
            'Provider Counter',
            style: TextStyle(
              fontSize: 30,
              fontWeight: FontWeight.bold,
            ),
          ),

          const SizedBox(height: 10),

          Text(
            '${counter.count}',
            style: const TextStyle(
              fontSize: 56,
            ),
          ),

          ElevatedButton(
            onPressed: counter.increment,
            child: const Text('Increment'),
          ),
        ],
      ),
    );
  }
}
