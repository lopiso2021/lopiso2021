import pandas as pd
from xarray import DataArray
import icclim
from netCDF4 import Dataset
import datetime
import os
pr = pd.read_csv(r'C:\Users\fekadu\OneDrive\Desktop\tmin17.csv') 
locs = ['A','B','C','D','E','F','G','H','I','J','K','L','M','N','O','P','Q']
times = pd.date_range('1990-01-01', periods=11323)
dt3 = datetime.datetime(1990,1,1)
dt4 = datetime.datetime(2020,12,31)
out_f = "TN90p_0101121_1990-2020.nc"
if os.path.exists(out_f):
    try:
        os.remove(out_f)
    except Exception as e:
            print(f"Error deleting existing file: {e}")
f = DataArray(pr, coords=[times, locs], dims=['time', 'space'])
try:
   icclim.index(
    index_name="TN90p",
    in_files=f,
    var_name=None,
    slice_mode=["season",[10,11,12,1]],
    base_period_time_range=[dt3,dt4],
    out_file=out_f,
)
except Exception as e:
            print(f"Error deleting existing file: {e}")
if os.path.exists(out_f):
  nc=Dataset(out_f)
  a = pd.DataFrame(nc.variables['TN90p'][:].data, columns=locs, index=pd.to_datetime(nc.variables['time'][:].data))
  a.to_csv('TN90p1011121season.csv', sep=',', header=False, index=False)
else:
    print(f"NetCDF file '{out_f}' not found.")
