# Flutter Architecture Guideline
<img src="architecture.png"/>

### It is the folder architecture created by my approach. It maybe useful with your contribution to improve architecture. Let's explain about each part more deeply.

> Keep in mind that, following numbered parts are folders.
--------

## 1. bloc
This is layer is used to handle logical operations. It maybe other state management too. It consists of 2 parts:

- Blocs

For each bloc, I create folder and it contains cubit or bloc, events and states.

```
- login
  - login_bloc (login_cubit)
  - login_event
  - login_state
```

- Bloc Observer (optional)
BlocObserver is simple delegate to handle all operations of Bloc in one place. You can learn about Bloc using [this link](https://www.youtube.com/watch?v=w6XWjpBK4W8&list=PLptHs0ZDJKt_T-oNj_6Q98v-tBnVf-S_o).
--------
## 2. config
Configrations are added to this folder. It contains the followings:

- config.dart
We can store main configrations in this file:
```dart
class Configs {
  Configs._();

  static const String SENTRY_DSN ='...';
  static const bool enableLogging = kDebugMode;
  static const String baseUrl = '...';
}
```
- init.dart
This file contains simple method that initializes all services and needed things before app started:
```dart
Future<void> init() async {
  WidgetsFlutterBinding.ensureInitialized();
  
  final locator = Locator.instance;
  final dio = Dio();
  
  if(Configs.enableLogging) {
    LoggingService(Configs.enableLogging);
    Bloc.observer = AppBlocObserver();
    dio.interceptors.add(LogInterceptorService());
  }
  
  locator.register<AuthService>(AuthService(dio));
   await locator.registerAsync<PreferencesService>(() async {
    final instance = PreferencesService.instance;
    await instance.init();
    return instance;
  });
}
```



