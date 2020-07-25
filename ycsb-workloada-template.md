
# Yahoo! Cloud System Benchmark
# Workload F: Read-modify-write workload
#   Application example: user database, where user records are read and modified by the user or to record user activity.
#
#   Read/read-modify-write ratio: 50/50
#   Default data size: 1 KB records (10 fields, 100 bytes each, plus key)
#   Request distribution: zipfian
recordcount=1000
operationcount=1000
workload=com.yahoo.ycsb.workloads.CoreWorkload
readallfields=true
readproportion=0.5
updateproportion=0
scanproportion=0
insertproportion=0
readmodifywriteproportion=0.5（默认值是0，指的是读取记录，修改它，写回来的操作比例）
requestdistribution=zipfian

workload=com.yahoo.ycsb.workloads.TimeSeriesWorkload
recordcount=1474560
operationcount=2949120
fieldlength=8（默认值是100，字段的大小）
fieldcount=64(默认值是10，记录中的字段数)
tagcount=4（每个时间系列的唯一标记组合数，如果此值为4，则每条记录将包含一个键和4个标记组合，例如A = A，B = A，C = A，D = A.）
tagcardinality=1,2,4,2（每个“度量”或字段的每个标记值的基数（唯一值的数量），以逗号分隔的列表。 每个值必须是从1到Java的Integer.MAX_VALUE的数字，并且必须有'tagcount'值。 如果有多于或少于'tagcount'的值，则忽略它或分别替换1。）
# A value from 0 to 0.999999 representing how sparse each time series
# should be. The higher this value, the greater the time interval between
# values in a single series. For example, if sparsity is 0 and there are
# 10 time series with a 'timestampinterval' of 60 seconds with a total
# time range of 10 intervals, you would see 100 values written, one per
# timestamp interval per time series. If the sparsity is 0.50 then there
# would be only about 50 values written so some time series would have
# missing values at each interval.
sparsity=0.0
# The percentage of time series that are "lagging" behind the current
# timestamp of the writer. This is used to mimic a common behavior where
# most sources (agents, sensors, etc) are writing data in sync (same timestamp)
# but a subset are running behind due to buffering, latency issues, etc.
delayedSeries=0.0
# The maximum amount of delay for delayed series in interval counts. The
# actual delay is chosen based on a modulo of the series index.
delayedIntervals=0
timestampunits=SECONDS（时间单位）
# The amount of time between each value in every time series in
# the units of 'timestampunits'.
timestampinterval=60
# The fixed or maximum amount of time added to the start time of a
# read or scan operation to generate a query over a range of time
# instead of a single timestamp. Units are shared with 'timestampunits'.
# For example if the value is set to 3600 seconds (1 hour) then
# each read would pick a random start timestamp based on the
#'insertstart' value and number of intervals, then add 3600 seconds
# to create the end time of the query. If this value is 0 then reads
# will only provide a single timestamp.
querytimespan=3600
readproportion=0.10
updateproportion=0.00

insertstart=0（第一个插入值的偏移量）
writeallfields=false（在更新的时候写所有字段）
fieldlengthdistribution=constant（字段长度分布形式，有constant，zipfian，uniform）insertorder=hashed（记录是按顺序插入还是伪随机插入，hashed还是ordered）
hotspotdatafraction=0.2（构成热集的数据项的百分比）
hotspotopnfraction=0.8（访问热集的操作百分比）
table=usertable（要对其运行查询的数据库表的名称）
#当measurementtype设置为raw时，测量将以以下csv格式输出为RAW数据点：“操作，测量的时间戳，我们的延迟”原始数据点在测试运行时收集在内存中。 每个数据点消耗大约50个字节（包括java对象开销）。 对于典型的100万到1000万次操作，大多数时候这应该适合存储器。 如果您计划每次运行执行数百万次操作，请考虑在使用RAW测量类型时配置具有更大RAM的计算机，或者将运行拆分为多次运行。
#（可选）可以指定输出文件以保存原始数据点。否则，原始数据点将写入stdout。如果输出文件已存在，将附加输出文件，否则将创建新的输出文件.measurement.raw.output_file =/tmp/ your_output_file_for_this_run
measurementtype=histogram（如何呈现延迟测量timeseries，histogram，raw）
measurement.histogram.verbose = false（使用直方图进行测量时是否发出单独的直方图桶）
histogram.buckets=1000(直方图中要跟踪的延迟范围（毫秒）)
timeseries.granularity=1000(时间序列的粒度（以毫秒为单位）)

--------------------------------------------------------------------
https://blog.csdn.net/clever_wr/java/article/details/88992723
