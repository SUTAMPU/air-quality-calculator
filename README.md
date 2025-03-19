# Air Quality Calculator
This project uses existing data from the __[PM2.5 Footprint Calculator v1.01](https://www.eg.mahidol.ac.th/dept/egce/pmfootprint/files/1_01/PM2.5%20Footprint%20Calculator%20Report%20v1.01_081221.pdf)__, developed by Mahidol University, Thailand.

The aim of this project is to raise awareness about individuals' PM2.5 emissions and encourage them to shift to greener modes of transportation. This can be achieved by integrating the project into AQI mobile applications. The following is an example interface of how the calculator would look like:
<br/><br/>
![example-interface](https://github.com/SUTAMPU/air-quality-calculator/blob/main/example.gif?raw=true)

# About the input
The passenger transport modes considered are:
- __Cars, pickups, public buses, and motorcycles__ —> emissions will depend on engine type and fuel type (Gasoline, B7, B20, LPG, CNG).
- __Skytrain__ —> emissions from energy production emissions.
```
if vehicle == "Skytrain":
  age = 1
  fuel = "Gasoline"
  distance = float(input("How many kilometers are you planning to travel today?\n - "))
  province = "Bangkok"
else:
  age = float(input("How old is your vehicle?\n - "))
  fuel = input("What type of fuel does it use? (Gasoline, B7, B20, CNG, LPG)\n - ")
  distance = float(input("How many kilometers are you planning to travel today?\n - "))
  province = input("In which province will you be traveling in?\n - ")
```
# About the output
The calculator categorises emissions into two types: Tank-to-Wheel (TtW) and Well-to-Wheel (WtW). Both emission types will be available in _calculation.py_ but _prediction.py_ will only account for WtW.
- __Tank-to-Wheel (TtW)__ —> considers only emissions from fuel combustion.
> TtW Emission = Vehicle Emission Factor × Activity Data
- __Well-to-Wheel (WtW)__ —> considers both fuel combustion emissions and energy production emissions.
> WtW Emission = (Vehicle Emission Factor × Activity Data) + (Fuel Production Emission Factor × Activity Data)
```
ttw = (vehicle_emission * distance) * pow(10, 6)
wtw = ((vehicle_emission  *distance) + (energy_production*distance)) * pow(10, 6)
```
# About the calculation
There are two types of calculation available: direct calculation and ML prediction. Both types utilises PySpark to account for large-scale data processing for future developments _(e.g., recommending best routes based on the current AQI, traffic rate, weather data, etc.)_:
### Direct Calculation (_calculation.py_)
This type filters out DataFrame based on inputs to find the _vehicle emission_ and _energy production_ rate. The values are then used to calculate the PM2.5 emissions.
```
ttw = (vehicle_emission*distance)*pow(10, 6)
wtw = ((vehicle_emission*distance)+(energy_production*distance))*pow(10, 6)
```
### ML Prediction (_prediction.py_)
This type predicts the PM2.5 emission rate without _vehicle emission_ and _energy production_ rate using RandomForestRegressor.
```
prediction = model.transform(input_df)
predicted_emission = prediction.select('Prediction').first()[0]
```
