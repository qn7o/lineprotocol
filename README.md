# lineprotocol

Export data to InfluxDB’s Line Protocol format based on specified input data structure (columns names) and desired output (measurement, fields, tags, timestamp).

Any data going through the exporter is sanitized according to Influx's recommendations:
https://docs.influxdata.com/influxdb/v1.0/write_protocols/line_protocol_tutorial/#data-types
https://docs.influxdata.com/influxdb/v1.0/write_protocols/line_protocol_tutorial/#special-characters-and-keywords

InfluxDB’s Line Protocol documentation can be found here:
https://docs.influxdata.com/influxdb/v1.0/write_protocols/


## Usage

from lineprotocol import LineProtocolExporter


input = [
    ['1', 'foo', 'x', '24', '50.2', 'True'],
    ['2', 'bar', 'y', '25', '17.0', 'True'],
    ['3', 'baz', 'z', '26', '100', 'False'],
]


exporter = LineProtocolExporter(
    labels=['id', 'name', 'some_letter', 'some_number', 'percentage', 'status'],
    measurement='example',
    tag_columns=['some_letter'],
    field_columns=['id', 'name', 'percentage', 'status'],
    field_types=['int', 'str', 'float', 'bool'],
    timestamp=1465839830100400200,
)

for line in input:
    print exporter.export(line)


# Output:
# example,some_letter=x id=1i,name="foo",percentage=50.2,status=True 1465839830100400200
# example,some_letter=y id=2i,name="bar",percentage=17.0,status=True 1465839830100400200
# example,some_letter=z id=3i,name="baz",percentage=100,status=False 1465839830100400200