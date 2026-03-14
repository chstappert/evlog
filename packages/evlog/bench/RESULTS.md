# Benchmark results

> Generated on 2026-03-14

## Bundle size

| Entry | Raw | Gzip |
|-------|----:|-----:|
| framework/nitro | 17.44 kB | 6.85 kB |
| logger | 11.88 kB | 3.64 kB |
| framework/next | 8.93 kB | 3.02 kB |
| adapter/sentry | 6.00 kB | 2.33 kB |
| adapter/otlp | 5.71 kB | 2.09 kB |
| enrichers | 6.15 kB | 1.92 kB |
| framework/sveltekit | 4.84 kB | 1.54 kB |
| adapter/posthog | 4.78 kB | 1.48 kB |
| adapter/fs | 3.38 kB | 1.42 kB |
| pipeline | 4.17 kB | 1.35 kB |
| utils | 3.20 kB | 1.34 kB |
| adapter/axiom | 3.24 kB | 1.30 kB |
| browser | 2.93 kB | 1.21 kB |
| error | 3.06 kB | 1.21 kB |
| framework/nestjs | 2.80 kB | 1.21 kB |
| adapter/better-stack | 2.62 kB | 1.08 kB |
| framework/elysia | 2.51 kB | 1.06 kB |
| framework/fastify | 2.29 kB | 1010 B |
| workers | 2.08 kB | 960 B |
| framework/express | 1.29 kB | 702 B |
| framework/hono | 1.07 kB | 593 B |
| toolkit | 486 B | 243 B |
| core (index) | 497 B | 205 B |
| types | 11 B | 31 B |
| **Total** | **101.33 kB** | **37.71 kB** |

## Comparison vs alternatives

> All loggers configured for JSON output to no-op destinations.
> See `bench/comparison/vs-alternatives.bench.ts` for methodology.

### simple string log

| Library | ops/sec | Mean | Relative |
|---------|--------:|-----:|---------:|
| consola | **2.64M** | 378ns | **fastest** |
| evlog | **1.65M** | 607ns | 1.60x slower |
| pino | **1.02M** | 984ns | 2.60x slower |
| winston | **930.7K** | 1.07µs | 2.84x slower |

### structured log (5 fields)

| Library | ops/sec | Mean | Relative |
|---------|--------:|-----:|---------:|
| consola | **1.77M** | 565ns | **fastest** |
| evlog | **1.34M** | 749ns | 1.33x slower |
| pino | **690.9K** | 1.45µs | 2.56x slower |
| winston | **411.9K** | 2.43µs | 4.30x slower |

### deep nested log

| Library | ops/sec | Mean | Relative |
|---------|--------:|-----:|---------:|
| evlog | **1.41M** | 708ns | **fastest** |
| consola | **1.04M** | 963ns | 1.36x slower |
| pino | **514.5K** | 1.94µs | 2.74x slower |
| winston | **217.8K** | 4.59µs | 6.48x slower |

### child / scoped logger

| Library | ops/sec | Mean | Relative |
|---------|--------:|-----:|---------:|
| evlog | **1.39M** | 722ns | **fastest** |
| pino | **868.5K** | 1.15µs | 1.60x slower |
| winston | **467.8K** | 2.14µs | 2.96x slower |
| consola | **271.1K** | 3.69µs | 5.11x slower |

### wide event lifecycle (evlog-native pattern)

| Library | ops/sec | Mean | Relative |
|---------|--------:|-----:|---------:|
| evlog | **902.4K** | 1.11µs | **fastest** |
| pino | **199.7K** | 5.01µs | 4.52x slower |
| winston | **116.6K** | 8.57µs | 7.74x slower |

### burst — 100 sequential logs

| Library | ops/sec | Mean | Relative |
|---------|--------:|-----:|---------:|
| consola | **40.7K** | 24.55µs | **fastest** |
| evlog | **13.8K** | 72.37µs | 2.95x slower |
| pino | **9.1K** | 110.26µs | 4.49x slower |
| winston | **6.4K** | 157.00µs | 6.39x slower |

### logger creation cost

| Library | ops/sec | Mean | Relative |
|---------|--------:|-----:|---------:|
| evlog | **18.84M** | 53ns | **fastest** |
| pino | **7.45M** | 134ns | 2.53x slower |
| winston | **5.49M** | 182ns | 3.43x slower |
| consola | **300.9K** | 3.32µs | 62.61x slower |

## Core benchmarks

### createUserAgentEnricher

| Benchmark | ops/sec | Mean | p99 | Samples |
|-----------|--------:|-----:|----:|--------:|
| no user-agent header | **12.22M** | 82ns | 120ns | 12,215,170 |
| Googlebot | **1.62M** | 616ns | 762ns | 1,624,216 |
| Firefox Linux | **1.44M** | 695ns | 1.10µs | 1,439,545 |
| Chrome desktop | **901.8K** | 1.11µs | 2.21µs | 901,782 |

### createGeoEnricher

| Benchmark | ops/sec | Mean | p99 | Samples |
|-----------|--------:|-----:|----:|--------:|
| Vercel headers (full) | **2.05M** | 487ns | 581ns | 2,052,098 |
| no geo headers | **1.21M** | 830ns | 1.20µs | 1,205,066 |
| Cloudflare headers (country only) | **453.0K** | 2.21µs | 4.05µs | 452,965 |

### createRequestSizeEnricher

| Benchmark | ops/sec | Mean | p99 | Samples |
|-----------|--------:|-----:|----:|--------:|
| with content-length | **9.11M** | 110ns | 190ns | 9,110,613 |
| no content-length | **7.97M** | 126ns | 170ns | 7,965,710 |

### createTraceContextEnricher

| Benchmark | ops/sec | Mean | p99 | Samples |
|-----------|--------:|-----:|----:|--------:|
| no trace headers | **6.40M** | 156ns | 210ns | 6,398,968 |
| with traceparent + tracestate | **3.18M** | 315ns | 381ns | 3,177,088 |
| with traceparent | **1.92M** | 522ns | 571ns | 1,917,175 |

### full enricher pipeline

| Benchmark | ops/sec | Mean | p99 | Samples |
|-----------|--------:|-----:|----:|--------:|
| all enrichers (no headers) | **920.5K** | 1.09µs | 1.36µs | 920,532 |
| all enrichers (all headers present) | **195.9K** | 5.10µs | 7.78µs | 195,933 |

### createError

| Benchmark | ops/sec | Mean | p99 | Samples |
|-----------|--------:|-----:|----:|--------:|
| string message | **107.3K** | 9.32µs | 18.04µs | 107,332 |
| with status | **106.6K** | 9.38µs | 17.96µs | 106,555 |
| full options | **104.7K** | 9.55µs | 18.36µs | 104,697 |
| with cause | **79.2K** | 12.62µs | 21.75µs | 79,249 |

### parseError

| Benchmark | ops/sec | Mean | p99 | Samples |
|-----------|--------:|-----:|----:|--------:|
| parse plain Error | **14.69M** | 68ns | 80ns | 14,685,462 |
| parse fetch-like error | **14.47M** | 69ns | 80ns | 14,473,011 |
| parse string | **13.87M** | 72ns | 100ns | 13,871,200 |
| parse EvlogError | **5.73M** | 175ns | 210ns | 5,729,148 |

### createError + parseError round-trip

| Benchmark | ops/sec | Mean | p99 | Samples |
|-----------|--------:|-----:|----:|--------:|
| create + parse (simple) | **109.5K** | 9.13µs | 17.84µs | 109,475 |
| create + parse (full) | **78.9K** | 12.68µs | 21.76µs | 78,893 |

### EvlogError serialization

| Benchmark | ops/sec | Mean | p99 | Samples |
|-----------|--------:|-----:|----:|--------:|
| toJSON() | **4.56M** | 219ns | 340ns | 4,559,876 |
| toString() | **1.44M** | 694ns | 902ns | 1,441,593 |
| JSON.stringify() | **679.8K** | 1.47µs | 1.73µs | 679,759 |

### JSON serialization (production mode)

| Benchmark | ops/sec | Mean | p99 | Samples |
|-----------|--------:|-----:|----:|--------:|
| emit + JSON.stringify | **292.7K** | 3.42µs | 6.93µs | 292,749 |

### pretty print (development mode)

| Benchmark | ops/sec | Mean | p99 | Samples |
|-----------|--------:|-----:|----:|--------:|
| emit + pretty print | **285.1K** | 3.51µs | 6.85µs | 285,143 |

### silent mode (no output)

| Benchmark | ops/sec | Mean | p99 | Samples |
|-----------|--------:|-----:|----:|--------:|
| emit silent (event build only) | **290.5K** | 3.44µs | 6.81µs | 290,501 |

### JSON.stringify baseline

| Benchmark | ops/sec | Mean | p99 | Samples |
|-----------|--------:|-----:|----:|--------:|
| raw JSON.stringify (same payload) | **608.5K** | 1.64µs | 2.61µs | 608,463 |

### createLogger

| Benchmark | ops/sec | Mean | p99 | Samples |
|-----------|--------:|-----:|----:|--------:|
| no initial context | **7.48M** | 134ns | 191ns | 7,481,503 |
| with shallow context | **7.46M** | 134ns | 190ns | 7,456,982 |
| with nested context | **7.02M** | 142ns | 230ns | 7,021,374 |

### createRequestLogger

| Benchmark | ops/sec | Mean | p99 | Samples |
|-----------|--------:|-----:|----:|--------:|
| with method + path | **7.48M** | 134ns | 210ns | 7,477,355 |
| with method + path + requestId | **5.00M** | 200ns | 241ns | 4,996,131 |

### log.set()

| Benchmark | ops/sec | Mean | p99 | Samples |
|-----------|--------:|-----:|----:|--------:|
| shallow merge (3 fields) | **5.57M** | 179ns | 260ns | 5,573,116 |
| shallow merge (10 fields) | **4.93M** | 203ns | 331ns | 4,926,979 |
| deep nested merge | **1.96M** | 511ns | 742ns | 1,956,813 |
| multiple sequential sets | **1.86M** | 539ns | 691ns | 1,855,865 |

### log.emit()

| Benchmark | ops/sec | Mean | p99 | Samples |
|-----------|--------:|-----:|----:|--------:|
| emit minimal event | **868.3K** | 1.15µs | 1.37µs | 868,260 |
| emit with context | **265.2K** | 3.77µs | 6.02µs | 265,240 |
| full lifecycle (create + set + emit) | **251.9K** | 3.97µs | 7.94µs | 251,854 |
| emit with error | **21.7K** | 45.98µs | 87.13µs | 21,746 |

### log.set() payload sizes

| Benchmark | ops/sec | Mean | p99 | Samples |
|-----------|--------:|-----:|----:|--------:|
| small payload (2 fields) | **651.6K** | 1.53µs | 1.75µs | 651,591 |
| medium payload (50 fields) | **56.1K** | 17.84µs | 28.05µs | 56,065 |
| large payload (200 nested fields) | **9.6K** | 104.55µs | 225.51µs | 9,564 |

### head sampling

| Benchmark | ops/sec | Mean | p99 | Samples |
|-----------|--------:|-----:|----:|--------:|
| with sampling rates | **227.3K** | 4.40µs | 8.31µs | 227,253 |
| no sampling configured | **156.6K** | 6.39µs | 12.58µs | 156,615 |

### tail sampling (shouldKeep)

| Benchmark | ops/sec | Mean | p99 | Samples |
|-----------|--------:|-----:|----:|--------:|
| status match | **14.53M** | 69ns | 80ns | 14,529,642 |
| no match (fast path) | **14.51M** | 69ns | 80ns | 14,509,709 |
| duration match | **14.47M** | 69ns | 80ns | 14,473,793 |
| path glob match | **14.32M** | 70ns | 110ns | 14,316,243 |

### head + tail sampling combined

| Benchmark | ops/sec | Mean | p99 | Samples |
|-----------|--------:|-----:|----:|--------:|
| full emit with sampling (likely sampled out) | **848.8K** | 1.18µs | 3.36µs | 848,840 |
| full emit with force-keep (tail sampling hit) | **375.1K** | 2.67µs | 5.69µs | 375,103 |
