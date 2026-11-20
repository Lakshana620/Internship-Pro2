import 'package:flutter/material.dart';

void main() => runApp(const TipApp());

class TipApp extends StatelessWidget {
  const TipApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      debugShowCheckedModeBanner: false,
      theme: ThemeData(colorSchemeSeed: Colors.indigo, useMaterial3: true),
      home: const TipPage(),
    );
  }
}

class TipPage extends StatefulWidget {
  const TipPage({super.key});

  @override
  State<TipPage> createState() => _TipPageState();
}

class _TipPageState extends State<TipPage> {
  double bill = 0;
  double tipPercent = 10;
  int people = 1;

  double get tip => bill * tipPercent / 100;
  double get total => bill + tip;
  double get perPerson => people > 0 ? total / people : 0;

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Tip Calculator')),
      body: Center(
        child: ConstrainedBox(
          constraints: const BoxConstraints(maxWidth: 400),
          child: SingleChildScrollView(
            padding: const EdgeInsets.all(16),
            child: Column(
              crossAxisAlignment: CrossAxisAlignment.start,
              children: [
                Card(
                  child: Padding(
                    padding: const EdgeInsets.all(16),
                    child: Column(
                      crossAxisAlignment: CrossAxisAlignment.start,
                      children: [
                        TextField(
                          keyboardType: const TextInputType.numberWithOptions(
                            decimal: true,
                          ),
                          decoration: const InputDecoration(
                            labelText: 'Bill amount',
                            prefixText: '₹ ',
                            border: OutlineInputBorder(),
                          ),
                          onChanged: (v) =>
                              setState(() => bill = double.tryParse(v) ?? 0),
                        ),
                        const SizedBox(height: 16),
                        Text(
                          'Tip: ${tipPercent.round()}%',
                          style: const TextStyle(
                            fontSize: 16,
                            fontWeight: FontWeight.w600,
                          ),
                        ),
                        Slider(
                          value: tipPercent,
                          min: 0,
                          max: 30,
                          divisions: 30,
                          label: '${tipPercent.round()}%',
                          onChanged: (v) => setState(() => tipPercent = v),
                        ),
                        Row(
                          children: [
                            const Text(
                              'People',
                              style: TextStyle(
                                fontSize: 16,
                                fontWeight: FontWeight.w600,
                              ),
                            ),
                            const Spacer(),
                            IconButton(
                              icon: const Icon(Icons.remove_circle_outline),
                              onPressed: () => setState(() {
                                if (people > 1) people--;
                              }),
                            ),
                            Text(
                              people.toString(),
                              style: const TextStyle(fontSize: 18),
                            ),
                            IconButton(
                              icon: const Icon(Icons.add_circle_outline),
                              onPressed: () => setState(() => people++),
                            ),
                          ],
                        ),
                      ],
                    ),
                  ),
                ),
                const SizedBox(height: 8),
                Card(
                  child: Padding(
                    padding: const EdgeInsets.all(16),
                    child: Column(
                      crossAxisAlignment: CrossAxisAlignment.start,
                      children: [
                        Text('Tip: ₹ ${tip.toStringAsFixed(2)}'),
                        const SizedBox(height: 4),
                        Text('Total: ₹ ${total.toStringAsFixed(2)}'),
                        const Divider(height: 24),
                        const Text('Per person'),
                        Text(
                          '₹ ${perPerson.toStringAsFixed(2)}',
                          style: const TextStyle(
                            fontSize: 32,
                            fontWeight: FontWeight.bold,
                          ),
                        ),
                      ],
                    ),
                  ),
                ),
              ],
            ),
          ),
        ),
      ),
    );
  }
}
