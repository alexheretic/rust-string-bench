# rust-string-bench
Benchmarks of various rust string libraries.

## Benches
Benches are run using random strings generated at **12**, **50** and **1000** byte lengths.

* `from &str`: Constructing from a `&str`
* `from &'static str`: Constructing from a `&str`
* `format!`: Formatting.
* `clone`: Cloning.
* `eq &str`: Equality check with a `&str`.
* `&str to_lowercase`: Converting to lowercase utf8 -> `Self`.
* `&str to_ascii_lowercase`: Converting to lowercase (ascii only) -> `Self`.
* `&str replace`: Replace `-` for `_` -> `Self`.

### Run
```sh
./run
```
Note: Install [critcmp](https://github.com/BurntSushi/critcmp) to show final comparison of benches.

### Crate notes
* std `String` does not natively support:
    - `from &'static str`: bench uses `&str` impl.
* std `Arc<str>` does not natively support:
    - `from &'static str`: bench uses `&str` impl.
    - `format!`: bench uses std + conversion.
    - `&str to_lowercase`: bench uses std + conversion.
    - `&str to_ascii_lowercase`: bench uses std + conversion.
    - `&str replace`: bench uses std + conversion.
* _compact_str_ does not currently natively support:
    - `&str to_ascii_lowercase`: bench uses std + conversion.
    - `&str replace`: bench uses std + conversion.
* _smartstring_ does not currently natively support:
    - `from &'static str`: bench uses `&str` impl.
    - `format!`: bench uses std + conversion.
    - `&str to_lowercase`: bench uses std + conversion.
    - `&str to_ascii_lowercase`: bench uses std + conversion.
    - `&str replace`: bench uses std + conversion.
* _tungstenite_ `Utf8Bytes` does not currently natively support:
    - `format!`: bench uses std + conversion.
    - `&str to_lowercase`: bench uses std + conversion.
    - `&str to_ascii_lowercase`: bench uses std + conversion.
    - `&str replace`: bench uses std + conversion.

## Results
_2025-11-10, rust 1.91.1, Arch Linux, AMD 5800X_

```
group                               compact_str             smartstring             smol_str                std_arc_str             std_string              utf8_bytes
-----                               -----------             -----------             --------                -----------             ----------              ----------
&str replace len=12                 2.25     15.5±0.00ns    2.90     20.0±0.08ns    1.00      6.9±0.02ns    2.61     18.0±0.10ns    1.54     10.6±0.02ns    1.89     13.0±0.13ns
&str to_ascii_lowercase len=12      2.43     19.6±0.02ns    2.78     22.5±0.02ns    1.00      8.1±0.01ns    2.13     17.2±0.03ns    1.24     10.1±0.02ns    1.93     15.6±0.01ns
&str to_lowercase self len=12       5.05    129.6±0.21ns    1.58     40.5±0.49ns    1.00     25.7±0.06ns    1.48     38.0±0.05ns    1.20     30.8±0.07ns    1.48     38.0±0.00ns
clone len=12                        1.00      0.5±0.00ns    1.94      0.9±0.00ns    9.23      4.2±0.01ns    6.15      2.8±0.04ns    14.32     6.5±0.01ns    14.57     6.6±0.00ns
eq &str len=12                      1.34      2.6±0.00ns    1.24      2.4±0.01ns    1.23      2.4±0.00ns    1.00      1.9±0.00ns    1.09      2.1±0.00ns    1.13      2.2±0.01ns
format! len=12                      1.47     42.2±0.32ns    1.67     47.9±0.08ns    1.00     28.7±0.00ns    1.52     43.7±0.32ns    1.25     36.0±0.00ns    1.50     43.1±0.06ns
from &'static str len=12            24.93     5.2±0.01ns    54.08    11.3±0.02ns    1.00      0.2±0.00ns    41.35     8.6±0.00ns    31.67     6.6±0.00ns    5.22      1.1±0.00ns
from &str len=12                    1.00      5.4±0.01ns    2.35     12.7±0.01ns    2.78     15.0±0.03ns    1.61      8.6±0.01ns    1.26      6.8±0.03ns    1.99     10.7±0.00ns

group                               compact_str             smartstring             smol_str                std_arc_str             std_string              utf8_bytes
-----                               -----------             -----------             --------                -----------             ----------              ----------
&str replace len=50                 1.02     20.9±0.01ns    1.37     28.2±0.10ns    1.40     28.7±0.07ns    1.31     26.9±0.01ns    1.00     20.6±0.38ns    1.12     23.0±0.14ns
&str to_ascii_lowercase len=50      1.78     19.6±0.03ns    1.74     19.1±0.03ns    2.52     27.7±0.06ns    1.77     19.4±0.05ns    1.00     11.0±0.02ns    1.45     16.0±0.02ns
&str to_lowercase self len=50       1.64     76.5±0.34ns    1.22     56.9±0.06ns    1.30     60.6±0.03ns    1.21     56.2±0.00ns    1.00     46.5±0.00ns    1.22     56.9±0.10ns
clone len=50                        2.83      7.9±0.00ns    2.74      7.7±0.00ns    1.07      3.0±0.03ns    1.00      2.8±0.00ns    2.14      6.0±0.00ns    2.32      6.5±0.00ns
eq &str len=50                      1.14      2.5±0.03ns    1.12      2.4±0.01ns    1.21      2.6±0.00ns    1.00      2.2±0.00ns    1.08      2.4±0.00ns    1.10      2.4±0.00ns
format! len=50                      1.27     76.0±0.16ns    1.12     67.3±0.30ns    1.13     67.7±0.18ns    1.09     65.3±0.04ns    1.00     60.0±0.64ns    1.03     61.7±0.05ns
from &'static str len=50            1.00      0.2±0.00ns    74.55    15.7±0.05ns    1.00      0.2±0.00ns    38.89     8.2±0.00ns    29.60     6.2±0.00ns    5.24      1.1±0.00ns
from &str len=50                    1.05      6.8±0.00ns    2.27     14.6±0.05ns    1.51      9.7±0.02ns    1.32      8.5±0.01ns    1.00      6.4±0.00ns    1.55     10.0±0.03ns

group                               compact_str             smartstring             smol_str                std_arc_str             std_string              utf8_bytes
-----                               -----------             -----------             --------                -----------             ----------              ----------
&str replace len=1000               1.42    290.2±0.13ns    1.49    303.7±1.20ns    1.00    204.2±0.12ns    1.46    299.0±0.22ns    1.30    266.0±0.43ns    1.33    271.0±1.27ns
&str to_ascii_lowercase len=1000    1.62     60.6±2.08ns    1.42     53.0±1.92ns    1.54     57.4±0.15ns    1.35     50.4±0.22ns    1.00     37.4±0.10ns    1.07     39.9±0.29ns
&str to_lowercase self len=1000     4.46    501.6±0.07ns    1.24    139.4±0.05ns    1.12    126.1±0.49ns    1.17    132.2±0.13ns    1.01    113.8±0.13ns    1.00    112.5±0.35ns
clone len=1000                      5.50     15.5±0.03ns    5.41     15.3±0.01ns    1.03      2.9±0.03ns    1.00      2.8±0.01ns    4.77     13.4±0.01ns    2.31      6.5±0.01ns
eq &str len=1000                    1.43     12.0±0.01ns    1.31     11.0±0.03ns    1.42     11.9±0.01ns    1.43     12.0±0.01ns    1.00      8.4±0.02ns    1.49     12.4±0.03ns
format! len=1000                    1.42    113.8±0.10ns    1.63    130.7±0.64ns    1.16     92.8±0.25ns    1.19     95.2±0.03ns    1.00     80.1±0.18ns    1.07     85.4±0.19ns
from &'static str len=1000          1.01      0.2±0.00ns    129.72    27.1±0.96ns   1.00      0.2±0.00ns    82.21    17.1±0.02ns    65.69    13.7±0.00ns    5.28      1.1±0.00ns
from &str len=1000                  1.00     13.4±0.05ns    2.06     27.5±0.81ns    1.29     17.2±0.00ns    1.32     17.6±0.03ns    1.10     14.8±0.06ns    1.34     17.9±0.00ns
```
