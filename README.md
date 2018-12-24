# webpack-chain

[![NPM version][npm-image]][npm-url]
[![NPM downloads][npm-downloads]][npm-url]
[![Build Status][travis-image]][travis-url]

Ӧ��һ����ʽAPI�����ɺͼ� 2-4�汾��webpack�����õ��޸ġ�

���ĵ���Ӧ��webpack-chain��v5�汾��������ǰ�İ汾������ģ�

* [v4 docs](https://github.com/neutrinojs/webpack-chain/tree/v4)
* [v3 docs](https://github.com/neutrinojs/webpack-chain/tree/v3)
* [v2 docs](https://github.com/neutrinojs/webpack-chain/tree/v2)
* [v1 docs](https://github.com/neutrinojs/webpack-chain/tree/v1)

_ע��: ��Ȼ webpack-chain ���㷺Ӧ����Neutrino�У�Ȼ�����������ȫ�������ɹ��κ���Ŀʹ�á�_

## ����

webpack �ĺ������õĴ������޸Ļ���һ����Ǳ�����ڴ���� JavaScript ������Ȼ��������õ�����Ŀ��˵���� OK �ģ������㳢�Կ���Ŀ������Щ����ʹ����к������޸ľͻ��Ļ��Ҳ�������Ϊ����Ҫ�����˽�ײ����Ľṹ�Խ�����Щ���ġ�

`webpack-chain` ����ͨ���ṩ����ʽ��˳��ʽ�� API �������޸�webpack ���á�API�� Key ���ֿ������û�ָ�����������ã��������� ����Ŀ�޸����÷�ʽ �ı�׼����

ͨ������ʾ�����Ը����׵ؽ�����һ�㡣

## ��װ

`webpack-chain` ��Ҫ Node.js v6.9�����߰汾.  
`webpack-chain` Ҳֻ�������������ʹ��webpack��2��3��4�汾�����ö���

�����ʹ��Yarn����npm����װ���������������������ѡһ�����У���

### **Yarn��ʽ**

```bash
yarn add --dev webpack-chain
```

### **npm��ʽ**

```bash
npm install --save-dev webpack-chain
```

## ����

���㰲װ�� `webpack-chain`�� ��Ϳ��Կ�ʼ����һ��webpack�����á� ���ڱ�ָ�ϣ����ǵ�ʾ���������� `webpack.config.js` ��λ��������Ŀ�ĸ�Ŀ¼��

```js
// ���� webpack-chain ģ�飬��ģ��������һ�����ڴ���һ��webpack����API�ĵ�һ���캯����
const Config = require('webpack-chain');

// �Ըõ�һ���캯������һ���µ�����ʵ��
const config = new Config();

// ����ʽAPI�ı�����
// ÿ��API�ĵ��ö�����ٶԴ洢���õĸ��ġ�

config
  // �޸� entry ����
  .entry('index')
    .add('src/index.js')
    .end()
  // �޸� output ����
  .output
    .path('dist')
    .filename('[name].bundle.js');

// ����һ�����������Ժ������޸Ĺ���
config.module
  .rule('lint')
    .test(/\.js$/)
    .pre()
    .include
      .add('src')
      .end()
    // �����Դ�������use (loaders)
    .use('eslint')
      .loader('eslint-loader')
      .options({
        rules: {
          semi: 'off'
        }
      });

config.module
  .rule('compile')
    .test(/\.js$/)
    .include
      .add('src')
      .add('test')
      .end()
    .use('babel')
      .loader('babel-loader')
      .options({
        presets: [
          ['@babel/preset-env', { modules: false }]
        ]
      });

// Ҳ���Դ���һ�������Ĳ��!
config
  .plugin('clean')
    .use(CleanPlugin, [['dist'], { root: '/dir' }]);

// ��������޸���ɵ�Ҫ��webpackʹ�õ����ö���
module.exports = config.toConfig();
```

��������Ҳ�ܼ򵥡������������� �� �ڴ��ݸ�webpack֮ǰ���� `.toConfig()` ���������õ�����webpackʹ�á�

```js
// webpack.core.js
const Config = require('webpack-chain');
const config = new Config();

// ��Ŀ�깲������
// Make configuration shared across targets
// ...

module.exports = config;

// webpack.dev.js
const config = require('./webpack.core');

// Dev-specific configuration
// ������������
// ...
module.exports = config.toConfig();

// webpack.prod.js
const config = require('./webpack.core');

// Production-specific configuration
// ������������
// ...
module.exports = config.toConfig();
```

## ChainedMap

webpack-chain �еĺ���API�ӿ�֮һ�� `ChainedMap`. һ�� `ChainedMap`�Ĳ���������JavaScript Map, Ϊ��ʽ�����������ṩ��һЩ������ ���һ�����Ա����һ�� `ChainedMap`, �������������µ�API�ͷ���:

**��������˵����������Щ���������� `ChainedMap` , ������ʽ������Щ������**

```js
// �� Map �Ƴ���������.
clear()
```

```js
// ͨ����ֵ�� Map �Ƴ���������.
// key: *
delete(key)
```

```js
// ��ȡ Map ����Ӧ����ֵ
// key: *
// returns: value
get(key)
```

```js
// ��ȡ Map ����Ӧ����ֵ
// �������Map�в����ڣ���ChainedMap�иü���ֵ�ᱻ����Ϊfn�ķ���ֵ.
// key: *
// fn: Function () -> value
// returns: value
getOrCompute(key, fn)
```

```js
// ����Map���Ѵ��ڵļ���ֵ
// key: *
// value: *
set(key, value)
```

```js
// Map���Ƿ����һ������ֵ���ض��������� ����
// key: *
// returns: Boolean
has(key)
```

```js
// ����Map���Ѵ洢������ֵ������
// returns: Array
values()
```

```js
// ����Map��ȫ�����õ�һ������, ���� ��������������ԣ�ֵ����Ӧ����ֵ��
// ���Map�ǿգ����� `undefined`
// ʹ�� `.before() �� .after()` ��ChainedMap, �򽫰�����������������
// returns: Object, undefined if empty
entries()
````

```js
// �ṩһ�����������������Ժ�ֵ��ӳ��� Map��
// ��Ҳ�����ṩһ��������Ϊ�ڶ��������Ա���Ժϲ����������ơ�
// obj: Object
// omit: Optional Array
merge(obj, omit)
```

```js
// �Ե�ǰ����������ִ�к�����
// handler: Function -> ChainedMap
  // һ����ChainedMapʵ����Ϊ���������ĺ���
batch(handler)
```

```js
// ����ִ��һ������ȥ��������
// condition: Boolean
// whenTruthy: Function -> ChainedMap
  // ������Ϊ�棬���ð�ChainedMapʵ����Ϊ��һ��������ĺ���
// whenFalsy: Optional Function -> ChainedMap
  // ������Ϊ�٣����ð�ChainedMapʵ����Ϊ��һ��������ĺ���
when(condition, whenTruthy, whenFalsy)
```

## ChainedSet

webpack-chain �еĺ���API�ӿ���һ���� `ChainedSet`. һ�� `ChainedSet`�Ĳ���������JavaScript Map, Ϊ��ʽ�����������ṩ��һЩ������ ���һ�����Ա����һ�� `ChainedSet`, �������������µ�API�ͷ���:

**��������˵����������Щ���������� `ChainedSet` , ������ʽ������Щ������**

```js
// ���/׷�� ��Setĩβλ��һ��ֵ.
// value: *
add(value)
```

```js
// ��� ��Set��ʼλ��һ��ֵ.
// value: *
prepend(value)
```

```js
// �Ƴ�Set��ȫ��ֵ.
clear()
```

```js
// �Ƴ�Set��һ��ָ����ֵ.
// value: *
delete(value)
```

```js
// ���Set���Ƿ����һ��ֵ.
// value: *
// returns: Boolean
has(value)
```

```js
// ����Set��ֵ������.
// returns: Array
values()
```

```js
// ���Ӹ��������鵽 Set β����
// arr: Array
merge(arr)
```

```js

// �Ե�ǰ����������ִ�к�����
// handler: Function -> ChainedSet
  // һ���� ChainedSet ʵ����Ϊ���������ĺ���
batch(handler)
```

```js
// ����ִ��һ������ȥ��������
// condition: Boolean
// whenTruthy: Function -> ChainedSet
  // ������Ϊ�棬���ð� ChainedSet ʵ����Ϊ��һ��������ĺ���
// whenFalsy: Optional Function -> ChainedSet
  // ������Ϊ�٣����ð� ChainedSet ʵ����Ϊ��һ��������ĺ���
when(condition, whenTruthy, whenFalsy)
```

## �ټǷ���

��������д���������� ʹ�����д����������ͬ�ļ��� ChainedMap ����һ��ֵ
����, `devServer.hot` ��һ���ټǷ���, �������������:

```js
// �� ChainedMap ������һ��ֵ���ټǷ���
devServer.hot(true);

// ����������Ч��:
devServer.set('hot', true);
```

һ���ټǷ����ǿ���ʽ�ģ���˵�����������ԭʵ���������������ʽʹ��

### ����

����һ���µ����ö���

```js
const Config = require('webpack-chain');

const config = new Config();
```

�ƶ���API�ĸ���㽫�ı��������޸ĵ����ݵ������ġ� �����ͨ�� `config`�ڴ����ö������û���ͨ������ `.end()` ���������ƶ�һ�� ʹ���ƻظ��ߵ������Ļ�����
�������ϤjQuery, �������� `.end()` ����ԭ�����ơ���������˵��������ȫ����API���ö����ڵ�ǰ�������з���APIʵ���� ����������Ը�����Ҫ���� ��ʽAPI����.  
�йض������ټǺ͵ͼ�������Ч���ض�ֵ����ϸ��Ϣ������� [webpack�ĵ���νṹ](https://webpack.js.org/configuration/) �е���Ӧ���ʡ�

```js
Config : ChainedMap
```

#### �����ټǷ���

```js
config
  .amd(amd)
  .bail(bail)
  .cache(cache)
  .devtool(devtool)
  .context(context)
  .externals(externals)
  .loader(loader)
  .mode(mode)
  .parallelism(parallelism)
  .profile(profile)
  .recordsPath(recordsPath)
  .recordsInputPath(recordsInputPath)
  .recordsOutputPath(recordsOutputPath)
  .stats(stats)
  .target(target)
  .watch(watch)
  .watchOptions(watchOptions)
```

#### ���� entryPoints

```js
// �ص� config.entryPoints : ChainedMap
config.entry(name) : ChainedSet

config
  .entry(name)
    .add(value)
    .add(value)

config
  .entry(name)
    .clear()

// �õͼ��� config.entryPoints:

config.entryPoints
  .get(name)
    .add(value)
    .add(value)

config.entryPoints
  .get(name)
    .clear()
```

#### ���� output: �ټ�����

```js
config.output : ChainedMap

config.output
  .auxiliaryComment(auxiliaryComment)
  .chunkFilename(chunkFilename)
  .chunkLoadTimeout(chunkLoadTimeout)
  .crossOriginLoading(crossOriginLoading)
  .devtoolFallbackModuleFilenameTemplate(devtoolFallbackModuleFilenameTemplate)
  .devtoolLineToLine(devtoolLineToLine)
  .devtoolModuleFilenameTemplate(devtoolModuleFilenameTemplate)
  .filename(filename)
  .hashFunction(hashFunction)
  .hashDigest(hashDigest)
  .hashDigestLength(hashDigestLength)
  .hashSalt(hashSalt)
  .hotUpdateChunkFilename(hotUpdateChunkFilename)
  .hotUpdateFunction(hotUpdateFunction)
  .hotUpdateMainFilename(hotUpdateMainFilename)
  .jsonpFunction(jsonpFunction)
  .library(library)
  .libraryExport(libraryExport)
  .libraryTarget(libraryTarget)
  .path(path)
  .pathinfo(pathinfo)
  .publicPath(publicPath)
  .sourceMapFilename(sourceMapFilename)
  .sourcePrefix(sourcePrefix)
  .strictModuleExceptionHandling(strictModuleExceptionHandling)
  .umdNamedDefine(umdNamedDefine)
```

#### ���� resolve��������: �ټǷ���

```js
config.resolve : ChainedMap

config.resolve
  .cachePredicate(cachePredicate)
  .cacheWithContext(cacheWithContext)
  .enforceExtension(enforceExtension)
  .enforceModuleExtension(enforceModuleExtension)
  .unsafeCache(unsafeCache)
  .symlinks(symlinks)
```

#### ���� resolve ����

```js
config.resolve.alias : ChainedMap

config.resolve.alias
  .set(key, value)
  .set(key, value)
  .delete(key)
  .clear()
```

#### ���� resolve modules

```js
config.resolve.modules : ChainedSet

config.resolve.modules
  .add(value)
  .prepend(value)
  .clear()
```

#### ���� resolve aliasFields

```js
config.resolve.aliasFields : ChainedSet

config.resolve.aliasFields
  .add(value)
  .prepend(value)
  .clear()
```

#### ���� resolve descriptionFields

```js
config.resolve.descriptionFields : ChainedSet

config.resolve.descriptionFields
  .add(value)
  .prepend(value)
  .clear()
```

#### ���� resolve extensions

```js
config.resolve.extensions : ChainedSet

config.resolve.extensions
  .add(value)
  .prepend(value)
  .clear()
```

#### ���� resolve mainFields

```js
config.resolve.mainFields : ChainedSet

config.resolve.mainFields
  .add(value)
  .prepend(value)
  .clear()
```

#### ���� resolve mainFiles

```js
config.resolve.mainFiles : ChainedSet

config.resolve.mainFiles
  .add(value)
  .prepend(value)
  .clear()
```

#### ���� resolveLoader

��ǰAPI `config.resolveLoader` ��ͬ�� ���� `config.resolve` ����������ã�

##### ���� resolveLoader moduleExtensions

```js
config.resolveLoader.moduleExtensions : ChainedSet

config.resolveLoader.moduleExtensions
  .add(value)
  .prepend(value)
  .clear()
```

##### ���� resolveLoader packageMains

```js
config.resolveLoader.packageMains : ChainedSet

config.resolveLoader.packageMains
  .add(value)
  .prepend(value)
  .clear()
```

#### ���� performance�����ܣ�: �ټǷ���

```js
config.performance : ChainedMap

config.performance
  .hints(hints)
  .maxEntrypointSize(maxEntrypointSize)
  .maxAssetSize(maxAssetSize)
  .assetFilter(assetFilter)
```

#### ���� optimizations���Ż���: �ټǷ���

```js
config.optimization : ChainedMap

config.optimization
  .concatenateModules(concatenateModules)
  .flagIncludedChunks(flagIncludedChunks)
  .mergeDuplicateChunks(mergeDuplicateChunks)
  .minimize(minimize)
  .namedChunks(namedChunks)
  .namedModules(namedModules)
  .nodeEnv(nodeEnv)
  .noEmitOnErrors(noEmitOnErrors)
  .occurrenceOrder(occurrenceOrder)
  .portableRecords(portableRecords)
  .providedExports(providedExports)
  .removeAvailableModules(removeAvailableModules)
  .removeEmptyChunks(removeEmptyChunks)
  .runtimeChunk(runtimeChunk)
  .sideEffects(sideEffects)
  .splitChunks(splitChunks)
  .usedExports(usedExports)
```

#### ���� optimization minimizers����С�Ż�����

```js
// �ص� config.optimization.minimizers
config.optimization
  .minimizer(name) : ChainedMap
```

#### ���� optimization minimizers: ���

_ע��: ��Ҫ�� `new` ȥ������С�Ż����������Ϊ�Ѿ�Ϊ�������ˡ�_

```js
config.optimization
  .minimizer(name)
  .use(WebpackPlugin, args)

// ����

config.optimization
  .minimizer('css')
  .use(OptimizeCSSAssetsPlugin, [{ cssProcessorOptions: { safe: true } }])

// Minimizer ���Ҳ���������ǵ�·��ָ�����Ӷ������ڲ�ʹ�ò����webpack���õ��������������� require s��
config.optimization
  .minimizer('css')
  .use(require.resolve('optimize-css-assets-webpack-plugin'), [{ cssProcessorOptions: { safe: true } }])

```

#### ���� optimization minimizers: �޸Ĳ���

```js
config.optimization
  .minimizer(name)
  .tap(args => newArgs)

// ����
config
  .minimizer('css')
  .tap(args => [...args, { cssProcessorOptions: { safe: false } }])
```

#### ���� optimization minimizers: �޸�ʵ��

```js
config.optimization
  .minimizer(name)
  .init((Plugin, args) => new Plugin(...args));
```

#### ���� optimization minimizers: �Ƴ�

```js
config.optimization.minimizers.delete(name)
```

#### ���ò��

```js
// �ص� config.plugins
config.plugin(name) : ChainedMap
```

#### ���ò��: ���

_ע��: ��Ҫ�� `new` ȥ�����������Ϊ�Ѿ�Ϊ�������ˡ�_

```js
config
  .plugin(name)
  .use(WebpackPlugin, args)

// ����
config
  .plugin('hot')
  .use(webpack.HotModuleReplacementPlugin);

// ���Ҳ���������ǵ�·��ָ�����Ӷ������ڲ�ʹ�ò����webpack���õ��������������� require s��
config
  .plugin('env')
  .use(require.resolve('webpack/lib/EnvironmentPlugin'), [{ 'VAR': false }]);
```

#### ���ò��: �޸Ĳ���

```js
config
  .plugin(name)
  .tap(args => newArgs)

// ����
config
  .plugin('env')
  .tap(args => [...args, 'SECRET_KEY']);
```

#### ���ò��: �޸�ʵ��

```js
config
  .plugin(name)
  .init((Plugin, args) => new Plugin(...args));
```

#### ���ò��: �Ƴ�

```js
config.plugins.delete(name)
```

#### ���ò��: ��֮ǰ����

ָ����ǰ���������Ӧ������һ��ָ�����֮ǰִ�У��㲻����ͬһ�������ͬʱʹ�� `.before()` �� `.after()`��  

```js
config
  .plugin(name)
    .before(otherName)

// ����
config
  .plugin('html-template')
    .use(HtmlWebpackTemplate)
    .end()
  .plugin('script-ext')
    .use(ScriptExtWebpackPlugin)
    .before('html-template');
```

#### Config plugins: ��֮�����

ָ����ǰ���������Ӧ������һ��ָ�����֮��ִ�У��㲻����ͬһ�������ͬʱʹ�� `.before()` �� `.after()`��  

```js
config
  .plugin(name)
    .after(otherName)

// ����
config
  .plugin('html-template')
    .after('script-ext')
    .use(HtmlWebpackTemplate)
    .end()
  .plugin('script-ext')
    .use(ScriptExtWebpackPlugin);
```

#### ���� resolve ���

```js
// �ص� config.resolve.plugins
config.resolve.plugin(name) : ChainedMap
```

#### ���� resolve ���: ���

_ע��: ��Ҫ�� `new` ȥ�����������Ϊ�Ѿ�Ϊ�������ˡ�_

```js
config.resolve
  .plugin(name)
  .use(WebpackPlugin, args)
```

#### Config resolve plugins: modify arguments

```js
config.resolve
  .plugin(name)
  .tap(args => newArgs)
```

#### Config resolve plugins: modify instantiation

```js
config.resolve
  .plugin(name)
  .init((Plugin, args) => new Plugin(...args))
```

#### Config resolve plugins: removing

```js
config.resolve.plugins.delete(name)
```

#### Config resolve plugins: ordering before

Specify that the current `plugin` context should operate before another named
`plugin`. You cannot use both `.before()` and `.after()` on the same resolve
plugin.

```js
config.resolve
  .plugin(name)
    .before(otherName)

// Example

config.resolve
  .plugin('beta')
    .use(BetaWebpackPlugin)
    .end()
  .plugin('alpha')
    .use(AlphaWebpackPlugin)
    .before('beta');
```

#### Config resolve plugins: ordering after

Specify that the current `plugin` context should operate after another named
`plugin`. You cannot use both `.before()` and `.after()` on the same resolve
plugin.

```js
config.resolve
  .plugin(name)
    .after(otherName)

// Example

config.resolve
  .plugin('beta')
    .after('alpha')
    .use(BetaWebpackTemplate)
    .end()
  .plugin('alpha')
    .use(AlphaWebpackPlugin);
```

#### ���� node

```js
config.node : ChainedMap

config.node
  .set('__dirname', 'mock')
  .set('__filename', 'mock');
```

#### ���� devServer

```js
config.devServer : ChainedMap
```

#### Config devServer allowedHosts

```js
config.devServer.allowedHosts : ChainedSet

config.devServer.allowedHosts
  .add(value)
  .prepend(value)
  .clear()
```

#### Config devServer: shorthand methods

```js
config.devServer
  .bonjour(bonjour)
  .clientLogLevel(clientLogLevel)
  .color(color)
  .compress(compress)
  .contentBase(contentBase)
  .disableHostCheck(disableHostCheck)
  .filename(filename)
  .headers(headers)
  .historyApiFallback(historyApiFallback)
  .host(host)
  .hot(hot)
  .hotOnly(hotOnly)
  .https(https)
  .inline(inline)
  .info(info)
  .lazy(lazy)
  .noInfo(noInfo)
  .open(open)
  .openPage(openPage)
  .overlay(overlay)
  .pfx(pfx)
  .pfxPassphrase(pfxPassphrase)
  .port(port)
  .progress(progress)
  .proxy(proxy)
  .public(public)
  .publicPath(publicPath)
  .quiet(quiet)
  .setup(setup)
  .socket(socket)
  .staticOptions(staticOptions)
  .stats(stats)
  .stdin(stdin)
  .useLocalIp(useLocalIp)
  .watchContentBase(watchContentBase)
  .watchOptions(watchOptions)
```

#### Config module

```js
config.module : ChainedMap
```

#### Config module: shorthand methods

```js
config.module : ChainedMap

config.module
  .noParse(noParse)
```

#### Config module rules: shorthand methods

```js
config.module.rules : ChainedMap

config.module
  .rule(name)
    .test(test)
    .pre()
    .post()
    .enforce(preOrPost)
```

#### Config module rules uses (loaders): creating

```js
config.module.rules{}.uses : ChainedMap

config.module
  .rule(name)
    .use(name)
      .loader(loader)
      .options(options)

// Example

config.module
  .rule('compile')
    .use('babel')
      .loader('babel-loader')
      .options({ presets: ['@babel/preset-env'] });
```

#### Config module rules uses (loaders): modifying options

```js
config.module
  .rule(name)
    .use(name)
      .tap(options => newOptions)

// Example

config.module
  .rule('compile')
    .use('babel')
      .tap(options => merge(options, {
        plugins: ['@babel/plugin-proposal-class-properties']
      }));
```

#### Config module rules oneOfs (conditional rules)

```js
config.module.rules{}.oneOfs : ChainedMap<Rule>

config.module
  .rule(name)
    .oneOf(name)

// Example

config.module
  .rule('css')
    .oneOf('inline')
      .resourceQuery(/inline/)
      .use('url')
        .loader('url-loader')
        .end()
      .end()
    .oneOf('external')
      .resourceQuery(/external/)
      .use('file')
        .loader('file-loader')
```

---

### �ϲ�����

webpack-chain ֧�ֽ�����ϲ�������ʵ������ʵ�������� webpack-chain ģʽ���ֵĲ��֡� ��ע�⣬�ⲻ�� webpack ���ö��󣬵��������ٽ�webpack���ö����ṩ��webpack-chain ��ƥ��������֮ǰ�������ת����

```js
config.merge({ devtool: 'source-map' });

config.get('devtool') // "source-map"
```

```js
config.merge({
  [key]: value,

  amd,
  bail,
  cache,
  context,
  devtool,
  externals,
  loader,
  mode,
  parallelism,
  profile,
  recordsPath,
  recordsInputPath,
  recordsOutputPath,
  stats,
  target,
  watch,
  watchOptions,

  entry: {
    [name]: [...values]
  },

  plugin: {
    [name]: {
      plugin: WebpackPlugin,
      args: [...args],
      before,
      after
    }
  },

  devServer: {
    [key]: value,

    clientLogLevel,
    compress,
    contentBase,
    filename,
    headers,
    historyApiFallback,
    host,
    hot,
    hotOnly,
    https,
    inline,
    lazy,
    noInfo,
    overlay,
    port,
    proxy,
    quiet,
    setup,
    stats,
    watchContentBase
  },

  node: {
    [key]: value
  },

  optimizations: {
    concatenateModules,
    flagIncludedChunks,
    mergeDuplicateChunks,
    minimize,
    minimizer,
    namedChunks,
    namedModules,
    nodeEnv,
    noEmitOnErrors,
    occurrenceOrder,
    portableRecords,
    providedExports,
    removeAvailableModules,
    removeEmptyChunks,
    runtimeChunk,
    sideEffects,
    splitChunks,
    usedExports,
  },

  performance: {
    [key]: value,

    hints,
    maxEntrypointSize,
    maxAssetSize,
    assetFilter
  },

  resolve: {
    [key]: value,

    alias: {
      [key]: value
    },
    aliasFields: [...values],
    descriptionFields: [...values],
    extensions: [...values],
    mainFields: [...values],
    mainFiles: [...values],
    modules: [...values],

    plugin: {
      [name]: {
        plugin: WebpackPlugin,
        args: [...args],
        before,
        after
      }
    }
  },

  resolveLoader: {
    [key]: value,

    alias: {
      [key]: value
    },
    aliasFields: [...values],
    descriptionFields: [...values],
    extensions: [...values],
    mainFields: [...values],
    mainFiles: [...values],
    modules: [...values],
    moduleExtensions: [...values],
    packageMains: [...values],

    plugin: {
      [name]: {
        plugin: WebpackPlugin,
        args: [...args],
        before,
        after
      }
    }
  },

  module: {
    [key]: value,

    rule: {
      [name]: {
        [key]: value,

        enforce,
        issuer,
        parser,
        resource,
        resourceQuery,
        test,

        include: [...paths],
        exclude: [...paths],

        oneOf: {
          [name]: Rule
        },

        use: {
          [name]: {
            loader: LoaderString,
            options: LoaderOptions,
            before,
            after
          }
        }
      }
    }
  }
})
```

### ��������

��ʹ�õ�����¹���ChainedMap��ChainedSet�������ʹ��ִ������������when��������ָ��һ�����ʽ when()������������ʵ�Ի�����ԡ�������ʽ����ʵ�ģ���ʹ�õ�ǰ����ʵ����ʵ�����õ�һ������������������ѡ���ṩ������Ϊ��ʱ���õĵڶ����������ú���Ҳ�ǵ�ǰ���ӵ�ʵ����

```js
// ʾ�������������ڼ����minify���
config
  .when(process.env.NODE_ENV === 'production', config => {
    config
      .plugin('minify')
      .use(BabiliWebpackPlugin);
  });
```

```js
// ����ֻ�������������������С�������������devtool��Դӳ��
config
  .when(process.env.NODE_ENV === 'production',
    config => config.plugin('minify').use(BabiliWebpackPlugin),
    config => config.devtool('source-map')
  );
```

### ������ɵ�����

������ʹ�ü�����ɵ�webpack����config.toString()���⽫�������õ��ַ������汾�����а������������÷��Ͳ����ע����ʾ��

``` js
config
  .module
    .rule('compile')
      .test(/\.js$/)
      .use('babel')
        .loader('babel-loader');

config.toString();


{
  module: {
    rules: [
      /* config.module.rule('compile') */
      {
        test: /\.js$/,
        use: [
          /* config.module.rule('compile').use('babel') */
          {
            loader: 'babel-loader'
          }
        ]
      }
    ]
  }
}

```

Ĭ������£�������ɵ��ַ���������Ҫ�ĺ����Ͳ��������ֱ������������webpack���á�Ϊ�����ɿ��õ����ã�������ͨ��__expression���������������������Զ��庯���Ͳ�����ַ�������ʽ��

``` js
class MyPlugin {}
MyPlugin.__expression = `require('my-plugin')`;

function myFunction () {}
myFunction.__expression = `require('my-function')`;

config
  .plugin('example')
    .use(MyPlugin, [{ fn: myFunction }]);

config.toString();

/*
{
  plugins: [
    new (require('my-plugin'))({
      fn: require('my-function')
    })
  ]
}
*/
```

ͨ����·��ָ���Ĳ����require()�Զ���������䣺

``` js
config
  .plugin('env')
    .use(require.resolve('webpack/lib/ProvidePlugin'), [{ jQuery: 'jquery' }])

config.toString();


{
  plugins: [
    new (require('/foo/bar/src/node_modules/webpack/lib/EnvironmentPlugin.js'))(
      {
        jQuery: 'jquery'
      }
    )
  ]
}
```

�������Ե���toString��̬����Config���Ա����ַ�����֮ǰ�޸����ö���

```js
Config.toString({
  ...config.toConfig(),
  module: {
    defaultRules: [
      {
        use: [
          {
            loader: 'banner-loader',
            options: { prefix: 'banner-prefix.txt' },
          },
        ],
      },
    ],
  },
})


{
  plugins: [
    /* config.plugin('foo') */
    new TestPlugin()
  ],
  module: {
    defaultRules: [
      {
        use: [
          {
            loader: 'banner-loader',
            options: {
              prefix: 'banner-prefix.txt'
            }
          }
        ]
      }
    ]
  }
}
```

[npm-image]: https://img.shields.io/npm/v/webpack-chain.svg
[npm-downloads]: https://img.shields.io/npm/dt/webpack-chain.svg
[npm-url]: https://www.npmjs.com/package/webpack-chain
[travis-image]: https://api.travis-ci.org/neutrinojs/webpack-chain.svg?branch=master
[travis-url]: https://travis-ci.org/neutrinojs/webpack-chain
