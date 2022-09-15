# ola

Inverter x2 Production tang dan Power nam ngang nhieu Weather Station Irradience: fdjfdlkdf Power Ratio: %

2020 1/1

2021 1 2 3 4 5 6 7 8 9 10 11 12 ngay 1

1/2022 day 1 => 30

1/6/2022 0h=> 24h

sơ đồ solar để ngay chỗ thành phần có trong hệ thống solar

các thông số trong này thêm hình có lưu trữ pin bức xạ mặt trời => năng lượng sinh ra từ đó => hiệu suất làm việc là bao nhiêu

kết quả trình bay trình bày quản lý được người dùng trình bày quản lý danh sách các site solar cho từng user 2user, mỗi user có 3 site trong từng site => thống kê.

30:00 15 null 30:01 16 1 30:02 17 1 5s 30:03 18 1 30:04 19 1

30:05 18 -1 30:06 17 -1 30:07 16 -1 30:08 15 -1 30:09 14 -1

30:10 15 1 30:11 16 1 30:12 17 1 30:13 18 1 30:14 19 1

30:15 18 -1 30:16 17 -1 30:17 16 -1 30:18 15 -1 30:19 14 -1

irradience tich phan irradiation 0 0 00 01

58 59 1:00 12

gom co nhung thanh phan gi

test thu server

dang xai nhung thanh phan nao

elasticsearch export NODE\_OPTIONS="--max-old-space-size=8192" scp -r ./build admin@95.111.202.121:/home/admin/deploy/solar-monitoring-app

const something = { Inverter: { Production: "", Power: "", }, Meter: { Production: "", Power: "", }, Station: { GHI: "", Irradiation: "", }, };

// Production: all mwh in lifetime /\* Yield: (h) e.g. 400kw peak: 400kwh 1 hour (typical) 1000kwh : yield: 1000/400

Production: { target: { id: "Meter", attr: "EnergyMeterProduction" }, },

Yield: { function: "Production / Capacity", }, "Power Ratio": { function: "Production / ...", }, \*/

const Site = { Capacity: { type: "fixed" }, Weather: { type: "fixed" }, Temperature: { type: "fixed" }, Irradiation: { target: { id: "Station", attr: "Irradiation" }, },

Production: { target: { id: "Meter", attr: "EnergyMeterProduction" }, },

Yield: { function: "Production / Capacity", }, "Power Ratio": { function: "Production / ...", }, };

const Chart1 = { Production: "First Last of 1 hour minus them", Irradiation: "Weather Station", };

const Chart2 = { "Active Power": { target: { id: "Meter", attr: "Power", }, }, Irradiance: { target: { id: "Station", attr: "GHI", }, }, };

const Inverter = { Yield: { function: "Production / Capacity", },

Capacity: "fixed", Production: "", "Power Ratio": { function: "Power / Capacity", }, };

const PowerMeter = { Production: { value: null }, Power: { value: null }, };

const WeatherStation = { GHI: { value: null, unit: "W/m2", }, Irradiation: { value: null, unit: "wh/m2", }, };
