```
class SplashView extends ConsumerStatefulWidget {
  const SplashView({
    super.key,
    required this.duration,
    this.onFinish,
  });
  final Duration duration;
  final VoidCallback? onFinish;
  @override
  ConsumerState<SplashView> createState() => _SplashViewState();
}

class _SplashViewState extends ConsumerState<SplashView>
    with SingleTickerProviderStateMixin {
  late final AnimationController _controller;
  late final Animation<double> _animation;
  List<ProfileData> profiles = [];
  @override
  void initState() {
    super.initState();

    _controller = AnimationController(
      vsync: this,
      duration: const Duration(seconds: 2),
    );
    _animation = CurvedAnimation(
      parent: _controller,
      curve: Curves.easeInOut,
    );
    _controller.forward();
    _controller.addStatusListener(
      (status) async {
        if (status == AnimationStatus.completed) {
          await Future.delayed(widget.duration);
          if (!context.mounted) return;
          if (widget.onFinish != null) widget.onFinish!();
        }
      },
    );
  }

  @override
  void dispose() {
    _controller.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      backgroundColor: Colors.white,
      body: Center(
        child: AnimatedBuilder(
            animation: _animation,
            builder: (context, snapshot) {
              return ScaleTransition(
                scale: _animation,
                alignment: Alignment.center,
                child: const Hero(
                  tag: 'logo',
                  child: Image(
                    image: AssetImage('assets/images/splash-icon.png'),
                    height: 300,
                    width: 300,
                  ),
                ),
              );
            }
        ),
      ),
    );
  }
}
```
