# flutter_math

[![Run Tests in Docker](https://github.com/zentence-reading/flutter_math/actions/workflows/dart.yml/badge.svg)](https://github.com/zentence-reading/flutter_math/actions/workflows/dart.yml)
[![License](https://img.shields.io/badge/License-Apache_2.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)


## ⚠ Fork

This is a fork of ([`simpleclub/flutter_math`](https://github.com/simpleclub/flutter_math)), which itself forked the original ([`flutter_math`](https://github.com/znjameswu/flutter_math)). It aims to address compatibility issues and provide updates as the previous forks appear unmaintained.

---

Math equation rendering in pure Dart & Flutter. 

This project aims to achieve maximum compatibility and fidelity with regard to the [KaTeX](https://github.com/KaTeX/KaTeX) project, while maintaining the performance advantage of Dart and Flutter.

The TeX parser is a Dart port of the KaTeX parser. There are only a few unsupported features and parsing differences compared to the original KaTeX parser. List of some unsupported features can be found [here](doc/unsupported.md).


## [Online Demo](https://znjameswu.github.io/flutter_math_demo/)


## Rendering Samples

`x = \frac{-b \pm \sqrt{b^2 - 4ac}}{2a}`

![Example1](doc/img/delta.png)

`i\hbar\frac{\partial}{\partial t}\Psi(\vec x,t) = -\frac{\hbar}{2m}\nabla^2\Psi(\vec x,t)+ V(\vec x)\Psi(\vec x,t)`

![Example2](doc/img/schrodinger.png)

`\hat f(\xi) = \int_{-\infty}^\infty f(x)e^{- 2\pi i \xi x}\mathrm{d}x`

![Example3](doc/img/fourier.png)


## Usage
The usage is straightforward. Just `Math.tex(r'\frac a b')`. There is also optional arguments of `TexParserSettings settings`, which corresponds to  Settings in KaTeX and support a subset of its features.

Display-style equations:
```dart
Math.tex(r'\frac a b', mathStyle: MathStyle.display) // Default
```

In-line equations
```dart
Math.tex(r'\frac a b', mathStyle: MathStyle.text)
```

The default size of the equation is obtained from the build context. If you wish to specify the size, you can use `textStyle`. Note: this parameter will also change how big 1cm/1pt/1inch is rendered on the screen. If you wish to specify the size of those absolute units, use `logicalPpi`

```dart
Math.tex(
  r'\frac a b',
  textStyle: TextStyle(fontSize: 42),
  // logicalPpi: MathOptions.defaultLogicalPpiFor(42),
)
```

If you would like to display custom styled error message, you should use `onErrorFallback` parameter. You can also process the errors in this function. But beware this function is called in build function.
```dart
Math.tex(
  r'\garbled $tring', 
  textStyle: TextStyle(color: Colors.green),
  onErrorFallback: (err) => Container(
    color: Colors.red,
    child: Text(err.messageWithType, style: TextStyle(color: Colors.yellow)),
  ),
)
```

If you wish to have more granularity dealing with equations, you can manually invoke the parser and supply AST into the widget.
```dart
SyntaxTree ast;
try {
  ast = SyntaxTree(greenRoot: TexParser(r'\frac a b', TexParserSettings()).parse());
} on ParseException catch (e) {
  // Handle my error here
}

Math(
  ast: ast,
  mathStyle: MathStyle.text,
  textStyle: TextStyle(fontSize: 42),
)
```


## [Line Breaking](doc/line_breaking.md)


## Testing

This project uses Docker to ensure a consistent testing environment across different machines, which is especially important for golden file tests.

**Prerequisites:**

* [Docker Desktop](https://www.docker.com/products/docker-desktop/) installed and running.

**Running All Tests:**

To run all tests, execute the following command from the root of the project directory in your terminal (e.g., PowerShell, bash):

```bash
# For PowerShell on Windows:
docker run --rm -it -v "$(pwd):/build" --workdir /build ghcr.io/cirruslabs/flutter:3.27.0 flutter test

# For bash/zsh (Linux/macOS/Git Bash):
docker run --rm -it -v "${PWD}:/build" --workdir /build ghcr.io/cirruslabs/flutter:3.27.0 flutter test
```

This command uses a specific Flutter version (3.27.0) inside a Linux container to run the tests.

**Updating Golden Files:**

If you make changes that affect widget rendering, you may need to update the golden image files. Run the appropriate command for your terminal:

```bash
# For PowerShell on Windows:
docker run --rm -it -v "$(pwd):/build" --workdir /build ghcr.io/cirruslabs/flutter:3.27.0 flutter test --update-goldens

# For bash/zsh (Linux/macOS/Git Bash):
docker run --rm -it -v "${PWD}:/build" --workdir /build ghcr.io/cirruslabs/flutter:3.27.0 flutter test --update-goldens
```

Commit the updated golden files along with your code changes. The test setup is configured to generate and check goldens within this specific Docker environment.


## Credits

This project is possible thanks to the inspirations and resources from [the KaTeX Project](https://katex.org/), [MathJax](www.mathjax.org), [Zefyr](https://github.com/memspace/zefyr), and [CaTeX](https://github.com/simpleclub/CaTeX).
