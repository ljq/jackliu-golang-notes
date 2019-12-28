# string


### Common stitching methods

* + = To stitch
* bytes.Buffer
* strings.Builder (> = go1.10)

### Performance comparison of stitching methods

* The method using + = is the slowest and has the largest performance consumption;
* The performance of Buffer and Builder is almost the same, Builder is slightly better than Buffer in memory usage;
* Officially recommended strings.Builder, advantages: good performance, clear code;