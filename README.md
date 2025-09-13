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
_2025-09-13, rust 1.89.0, Arch Linux, AMD 5800X_

```
group                               compact_str             smartstring              smol_str                std_arc_str             std_string              utf8_bytes
-----                               -----------             -----------              --------                -----------             ----------              ----------
&str replace len=12                 1.47     15.9±0.03ns    1.69     18.2±0.04ns     2.28     24.5±0.04ns    1.67     18.0±0.01ns    1.00     10.8±0.03ns    1.22     13.1±0.03ns
&str to_ascii_lowercase len=12      1.98     19.7±0.03ns    2.23     22.1±0.04ns     1.38     13.7±0.01ns    1.89     18.8±0.02ns    1.00      9.9±0.01ns    1.45     14.4±0.00ns
&str to_lowercase self len=12       4.14    127.3±0.18ns    1.29     39.7±0.17ns     2.16     66.5±0.02ns    1.23     37.8±0.29ns    1.00     30.7±0.02ns    1.24     38.1±0.11ns
clone len=12                        1.00      0.4±0.00ns    2.46      1.1±0.00ns     9.35      4.2±0.01ns    6.04      2.7±0.01ns    13.69     6.1±0.06ns    14.58     6.5±0.00ns
eq &str len=12                      1.33      2.6±0.01ns    1.23      2.4±0.01ns     1.11      2.2±0.00ns    1.00      2.0±0.00ns    1.09      2.1±0.00ns    1.01      2.0±0.00ns
format! len=12                      1.39     39.8±0.02ns    1.47     42.3±0.12ns     1.00     28.7±0.08ns    1.51     43.2±0.04ns    1.25     35.9±0.15ns    1.52     43.6±0.13ns
from &'static str len=12            17.01     5.3±0.00ns    40.55    12.7±0.01ns     1.00      0.3±0.00ns    27.43     8.6±0.00ns    21.15     6.6±0.00ns    4.17      1.3±0.00ns
from &str len=12                    1.00      5.5±0.01ns    2.10     11.6±0.07ns     2.49     13.8±0.06ns    1.54      8.5±0.01ns    1.19      6.6±0.03ns    1.80     10.0±0.03ns
 
group                               compact_str             smartstring              smol_str                std_arc_str             std_string              utf8_bytes
-----                               -----------             -----------              --------                -----------             ----------              ----------
&str replace len=50                 1.02     20.2±0.07ns    1.40     27.8±0.10ns     4.37     86.6±0.18ns    1.31     26.0±0.04ns    1.00     19.8±0.10ns    1.13     22.3±0.07ns
&str to_ascii_lowercase len=50      1.89     20.2±0.01ns    1.83     19.6±0.05ns     9.86    105.4±0.08ns    1.88     20.1±0.01ns    1.00     10.7±0.02ns    1.38     14.8±0.03ns
&str to_lowercase self len=50       1.62     76.4±0.12ns    1.21     57.0±0.06ns     7.68    363.1±0.56ns    1.16     55.0±0.12ns    1.00     47.3±0.02ns    1.15     54.3±0.06ns
clone len=50                        3.00      8.1±0.00ns    2.76      7.4±0.01ns     1.11      3.0±0.06ns    1.00      2.7±0.04ns    2.25      6.1±0.11ns    2.43      6.5±0.02ns
eq &str len=50                      1.13      2.4±0.00ns    1.13      2.4±0.01ns     1.21      2.6±0.00ns    1.00      2.2±0.00ns    1.08      2.3±0.00ns    1.06      2.3±0.00ns
format! len=50                      1.30     72.3±0.06ns    1.15     63.7±0.17ns     1.24     68.8±0.13ns    1.12     62.0±0.07ns    1.00     55.6±0.41ns    1.19     66.1±0.11ns
from &'static str len=50            1.00      0.2±0.00ns    68.80    14.5±0.01ns     1.49      0.3±0.00ns    38.74     8.2±0.01ns    29.69     6.3±0.00ns    6.21      1.3±0.00ns
from &str len=50                    1.05      6.7±0.01ns    2.26     14.5±0.04ns     1.57     10.1±0.08ns    1.28      8.2±0.00ns    1.00      6.4±0.00ns    1.62     10.4±0.02ns
 
group                               compact_str             smartstring              smol_str                std_arc_str             std_string              utf8_bytes
-----                               -----------             -----------              --------                -----------             ----------              ----------
&str replace len=1000               1.02    268.0±0.09ns    1.08    283.0±0.10ns     2.25    591.3±0.48ns    1.08    282.5±0.15ns    1.00    262.5±0.20ns    1.03    270.2±0.17ns
&str to_ascii_lowercase len=1000    1.62     59.4±0.28ns    1.52     55.7±0.23ns     38.81  1424.3±2.79ns    1.46     53.6±0.03ns    1.00     36.7±0.03ns    1.07     39.3±0.13ns
&str to_lowercase self len=1000     4.89    497.8±0.72ns    1.46    148.3±0.25ns     57.12     5.8±0.01µs    1.25    127.6±0.15ns    1.00    101.9±0.33ns    1.35    137.9±0.40ns
clone len=1000                      5.49     15.7±0.04ns    5.48     15.6±0.02ns     1.04      3.0±0.02ns    1.00      2.9±0.04ns    3.71     10.6±0.01ns    2.27      6.5±0.01ns
eq &str len=1000                    1.37     11.9±0.04ns    1.42     12.3±0.01ns     1.33     11.6±0.01ns    1.42     12.3±0.09ns    1.00      8.7±0.07ns    1.43     12.4±0.03ns
format! len=1000                    1.31    115.6±0.46ns    1.53    135.0±0.46ns     1.23    108.6±0.24ns    1.05     92.9±0.44ns    1.06     93.9±0.15ns    1.00     88.2±0.01ns
from &'static str len=1000          1.00      0.2±0.00ns    134.18    28.2±0.26ns    1.50      0.3±0.00ns    81.40    17.1±0.01ns    50.96    10.7±0.01ns    6.24      1.3±0.00ns
from &str len=1000                  1.01     11.5±0.01ns    2.53     28.7±0.08ns     1.51     17.1±0.02ns    1.52     17.2±0.01ns    1.00     11.3±0.01ns    1.57     17.8±0.07ns
```
