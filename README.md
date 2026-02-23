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

### Relevant crate features

| | std `String` | std `Arc<str>` | _smol_str_ | _compact_str_ | _smartstring_ | _tungstenite_ `Utf8Bytes`
|---|---|---|---|---|---|---
|**from &'static str**|:black_circle:|:black_circle:|:white_check_mark:|:white_check_mark:|:black_circle:|:white_check_mark:
|**format!**|:white_check_mark:|:large_blue_circle:|:white_check_mark:|:white_check_mark:|:large_blue_circle:|:large_blue_circle:
|**&str to_\*case**|:white_check_mark:|:large_blue_circle:|:white_check_mark:|:white_check_mark:|:large_blue_circle:|:large_blue_circle:
|**&str to_ascii_\*case**|:white_check_mark:|:large_blue_circle:|:white_check_mark:|:large_blue_circle:|:large_blue_circle:|:large_blue_circle:
|**&str replace**|:white_check_mark:|:large_blue_circle:|:white_check_mark:|:large_blue_circle:|:large_blue_circle:|:large_blue_circle:

**Legend**
* :white_check_mark: Supported
* :large_blue_circle: Uses std + conversion / allocates / probably suboptimal
* :black_circle: Not supported / allocates / suboptimal


## Results
_2026-02-23, rust 1.93.1, Arch Linux, AMD 5800X_

```
group                             compact_str             smartstring             smol_str                std_arc_str             std_string              utf8_bytes
-----                             -----------             -----------             --------                -----------             ----------              ----------
&str replace len=12               2.19     15.8±0.01ns    2.79     20.1±0.11ns    1.00      7.2±0.01ns    2.44     17.6±0.01ns    1.50     10.8±0.05ns    1.84     13.3±0.13ns
&str to_ascii_lowercase len=12    1.97     19.9±0.10ns    2.24     22.6±0.05ns    1.00     10.1±0.00ns    1.88     19.0±0.13ns    1.04     10.5±0.04ns    1.55     15.7±0.02ns
&str to_lowercase self len=12     4.83    129.6±0.27ns    1.47     39.5±0.48ns    1.00     26.9±0.09ns    1.41     37.9±0.04ns    1.22     32.8±0.02ns    1.39     37.4±0.20ns
clone len=12                      1.00      0.5±0.00ns    1.99      0.9±0.00ns    1.94      0.9±0.00ns    6.14      2.8±0.00ns    14.33     6.5±0.02ns    15.08     6.8±0.00ns
eq &str len=12                    1.36      2.6±0.01ns    1.35      2.6±0.00ns    1.34      2.6±0.00ns    1.01      2.0±0.00ns    1.00      1.9±0.00ns    1.04      2.0±0.01ns
format! len=12                    1.36     44.4±0.48ns    1.48     48.2±0.40ns    1.00     32.7±0.09ns    1.41     46.0±0.20ns    1.24     40.4±0.03ns    1.48     48.3±0.29ns
from &'static str len=12          25.73     5.4±0.01ns    53.86    11.3±0.00ns    1.00      0.2±0.00ns    34.05     7.2±0.03ns    32.15     6.8±0.01ns    5.23      1.1±0.01ns
from &str len=12                  1.00      5.4±0.01ns    2.46     13.4±0.02ns    2.52     13.7±0.04ns    1.36      7.4±0.01ns    1.32      7.2±0.00ns    1.84     10.0±0.36ns

group                             compact_str             smartstring             smol_str                std_arc_str             std_string              utf8_bytes
-----                             -----------             -----------             --------                -----------             ----------              ----------
&str replace len=50               1.04     20.8±0.12ns    1.43     28.6±0.28ns    1.48     29.5±0.09ns    1.32     26.3±0.01ns    1.00     20.0±0.01ns    1.16     23.2±0.08ns
&str to_ascii_lowercase len=50    1.41     20.2±0.09ns    1.31     18.7±0.06ns    1.95     27.8±0.01ns    1.37     19.6±0.04ns    1.00     14.3±0.04ns    1.13     16.1±0.04ns
&str to_lowercase self len=50     1.54     82.4±0.33ns    1.22     65.2±0.55ns    1.26     67.4±0.07ns    1.15     61.5±0.09ns    1.00     53.6±0.02ns    1.11     59.4±0.30ns
clone len=50                      2.86      7.9±0.09ns    2.82      7.8±0.04ns    2.06      5.7±0.02ns    1.00      2.8±0.08ns    2.20      6.1±0.00ns    2.44      6.8±0.01ns
eq &str len=50                    2.26      4.9±0.00ns    1.13      2.5±0.00ns    1.21      2.6±0.00ns    1.00      2.2±0.00ns    1.00      2.2±0.00ns    1.04      2.2±0.05ns
format! len=50                    1.27     78.3±0.24ns    1.31     80.5±0.16ns    1.33     81.6±0.03ns    1.18     72.6±0.31ns    1.00     61.6±0.33ns    1.22     75.1±0.03ns
from &'static str len=50          1.01      0.2±0.00ns    73.05    15.3±0.00ns    1.00      0.2±0.00ns    32.29     6.8±0.00ns    31.38     6.6±0.02ns    5.21      1.1±0.01ns
from &str len=50                  1.02      6.9±0.04ns    2.54     17.2±0.10ns    1.39      9.4±0.01ns    1.03      7.0±0.01ns    1.00      6.8±0.01ns    1.52     10.3±0.01ns

group                             compact_str             smartstring             smol_str                std_arc_str             std_string              utf8_bytes
-----                             -----------             -----------             --------                -----------             ----------              ----------
&str replace len=1000             1.25    269.2±0.92ns    1.33    287.8±1.59ns    1.00    216.0±1.73ns    1.31    282.8±0.21ns    1.26    272.4±1.28ns    1.29    278.1±0.52ns
&str to_ascii_lowercase len=1000  1.47     55.8±0.98ns    1.48     55.8±1.07ns    1.46     55.3±0.60ns    1.41     53.5±0.46ns    1.00     37.8±0.07ns    1.08     40.8±0.12ns
&str to_lowercase self len=1000   4.14    502.0±0.83ns    1.27    154.3±0.62ns    1.18    143.3±0.31ns    1.17    142.5±0.31ns    1.10    133.0±0.24ns    1.00    121.3±1.38ns
clone len=1000                    5.17     15.8±0.07ns    5.43     16.6±0.01ns    1.87      5.7±0.01ns    1.00      3.1±0.03ns    3.48     10.6±0.02ns    2.23      6.8±0.03ns
eq &str len=1000                  1.35     11.8±0.08ns    1.26     11.0±0.05ns    1.18     10.4±0.01ns    1.39     12.2±0.01ns    1.00      8.8±0.03ns    1.44     12.6±0.04ns
format! len=1000                  1.17     96.6±0.34ns    1.44    118.7±0.65ns    1.21     99.1±1.08ns    1.19     97.9±0.14ns    1.00     82.3±0.12ns    1.20     98.9±0.21ns
from &'static str len=1000        1.01      0.2±0.00ns    139.22    29.4±0.11ns   1.00      0.2±0.00ns    61.31    13.0±0.01ns    64.65    13.7±0.00ns    5.19      1.1±0.00ns
from &str len=1000                1.00     12.5±0.03ns    2.44     30.6±0.42ns    1.36     17.0±0.01ns    1.04     13.1±0.03ns    1.12     14.0±0.00ns    1.54     19.3±0.01ns
```
