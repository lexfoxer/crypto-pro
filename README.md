> Fork пакета [vgoma/crypto-pro](https://github.com/vgoma/crypto-pro).
>
> Основные отличия от оригинального пакета:
> - подняты версии пакетов,
> - реализован "[indirect eval](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/eval#direct_and_indirect_eval)"
> - удалены примеры
>
> TODO:
> - [ ] Починить тесты

---

# cryptoPro
Единое, асинхронное API для взаимодействия с КриптоПРО ЭЦП Browser Plug-In.

- [Зачем мне этот пакет?](#why)
- [Установка](#install)
- [API](#api)
    - [Методы объекта cryptoPro](#api-cryptopro)
    - [Методы объекта сертификата](#api-certificate)

<a name="why"></a>
## Зачем мне этот пакет?
КриптоПРО ЭЦП Browser Plug-In доступен в разных браузерах в двух версиях.
Асинхронной (в современных браузерах) и синхронной (в браузерах постарше).
С помощью этого пакета можно не писать реализацию под каждую версию плагина дважды.
И вместо [этого](http://cpdn.cryptopro.ru/content/cades/plugin-samples-sign.html)
и [этого](http://cpdn.cryptopro.ru/content/cades/plugin-samples-sign-cades-bes-async.html)
написать это (UMD):

![example-umd.gif](resources/example-umd-js.gif)

или это (ES Modules + Typescript):

![example-esm.gif](resources/example-esm-ts.gif)

<a name="install"></a>
## Установка
Для NPM:
```bash
npm install @lexfoxer/crypto-pro
```

Для Yarn:
```bash
yarn add @lexfoxer/crypto-pro
```

Подключение пакета как UMD модуля через тэг script:
```html
<script src="crypto-pro/dist/crypto-pro.min.js"></script>
<script>
window.cryptoPro.getUserCertificates()
  .then(function (certificates) {
    //...
  })
  .catch(function (error) {
    //...
  });
</script>
```

Подключение пакета как ES модуля с Typescript или JavaScript:
```typescript
import { getUserCertificates, Certificate } from 'crypto-pro';

(async () => {
  let certificates: Certificate[];

  try {
    certificates = await getUserCertificates();
  } catch(error) {
    // ...
  }
})();
```

Список требуемых полифиллов (если необходимы, подключаются самостоятельно):
- Promise
- Array.prototype.find

<a name="api"></a>
## API

<a name="api-cryptopro"></a>
### Методы объекта cryptoPro
- [getUserCertificates](src/api/getUserCertificates.ts) - возвращает список [сертификатов](#api-certificate), доступных пользователю в системе
- [getCertificate](src/api/getCertificate.ts) - возвращает [сертификат](#api-certificate) по отпечатку
- [createAttachedSignature](src/api/createAttachedSignature.ts) - создает совмещенную (присоединенную) подпись сообщения
- [createDetachedSignature](src/api/createDetachedSignature.ts) - создает отсоединенную (открепленную) подпись сообщения
- [createXMLSignature](src/api/createXMLSignature.ts) - создает XML подпись для документа в формате XML
- [createHash](src/api/createHash.ts) - создает хеш сообщения по ГОСТ Р 34.11-2012 256 бит
- [createSignature](src/api/createSignature.ts) - создает подпись сообщения
    > Является устаревшим и будет убран из будущих версий.
    Используйте "createAttachedSignature" и "createDetachedSignature".
- [getSystemInfo](src/api/getSystemInfo.ts) - возвращает информацию о CSP и плагине
- [isValidSystemSetup](src/api/isValidSystemSetup.ts) - возвращает флаг корректности настроек ЭП на машине
- [execute](src/api/execute.ts) - компилирует и выполняет переданную функцию для доступной браузерной среды (синхронной/асинхронной)

<a name="api-certificate"></a>
### Методы объекта сертификата
[Сертификат](src/api/certificate/certificate.ts) предоставляет следущее API:
- [isValid](src/api/certificate/isValid.ts) - возвращает флаг действительности сертификата
- [getCadesProp](src/api/certificate/getCadesProp.ts) - возвращает указанное внутренее свойство у сертификата в формате Cades
- [exportBase64](src/api/certificate/exportBase64.ts) - возвращает сертификат в формате base64
- [getAlgorithm](src/api/certificate/getAlgorithm.ts) - возвращает информацию об алгоритме сертификата
- [getOwnerInfo](src/api/certificate/getInfo.ts) - возвращает расшифрованную информацию о владельце сертификата
- [getIssuerInfo](src/api/certificate/getInfo.ts) - возвращает расшифрованную информацию об издателе сертификата
- [getExtendedKeyUsage](src/api/certificate/getExtendedKeyUsage.ts) - возвращает ОИД'ы сертификата
- [getDecodedExtendedKeyUsage](src/api/certificate/getDecodedExtendedKeyUsage.ts) - возвращает расшифрованные ОИД'ы
- [hasExtendedKeyUsage](src/api/certificate/hasExtendedKeyUsage.ts) - проверяет наличие ОИД'а (ОИД'ов) у сертификата
