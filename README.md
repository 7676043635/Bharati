import math
import random

toll_amount1 = 0.0
toll_amount = 0.0

# Define the classes
class Vehicle:
    def __init__(self, vehicle_type, license_plate):
        self.vehicle_type = vehicle_type
        self.license_plate = license_plate

class TollBooth:
    def __init__(self, toll_rate):
        self.toll_rate = toll_rate

    def collect_toll(self, vehicle):
        global toll_amount
        if vehicle.vehicle_type == "Car":
            toll_amount = self.toll_rate
        elif vehicle.vehicle_type == "Ambulance":
            toll_amount = 0
        elif vehicle.vehicle_type == "SUV":
            toll_amount = self.toll_rate * 2
        elif vehicle.vehicle_type == "Truck":
            toll_amount = self.toll_rate * 3
        else:
            toll_amount = 0

        print(f"Vehicle license plate is :{vehicle.license_plate}")

class DummyAccount:
    def __init__(self, balance):
        self.balance = balance
    
    def initiate_money(self, amount):
        self.balance += amount
    
    def deduct_money(self, amount):
        if self.balance >= amount:
            self.balance -= amount
        else:
            print("Insufficient balance.")

def calculate_distance(lat1, lon1, lat2, lon2):
    # Convert latitude and longitude from degrees to radians
    lat1 = math.radians(lat1)
    lon1 = math.radians(lon1)
    lat2 = math.radians(lat2)
    lon2 = math.radians(lon2)

    # Haversine formula
    dlon = lon2 - lon1
    dlat = lat2 - lat1
    a = math.sin(dlat/2)**2 + math.cos(lat1) * math.cos(lat2) * math.sin(dlon/2)**2
    c = 2 * math.atan2(math.sqrt(a), math.sqrt(1-a))
    distance = 6371 * c  # Radius of the Earth in kilometers

    return distance

# Simulation functions
def calculate_speed_code(distance, time):
    speed_kmh = (distance / time) * 3600
    if speed_kmh < 60:
        return "Speed is less than 60 km/h"
    elif speed_kmh < 100:
        return "Speed is less than 100 km/h"
    elif speed_kmh < 120:
        return "Speed is less than 120 km/h"
    elif speed_kmh < 140:
        return "Speed is less than 140 km/h"
    else:
        return "Speed is greater than 140 km/h"

def check_congestion_level(vehicle_type):
    if vehicle_type == "Car":
        return random.uniform(0, 1)
    elif vehicle_type == "Truck":
        return random.uniform(0, 1)
    elif vehicle_type == "SUV":
        return random.uniform(0, 1)
    elif vehicle_type == "Ambulance":
        return random.uniform(0, 1)
    else:
        return None

# Example usage
print("Enter the vehicle latitude and longitude:")
vehicle_lat = float(input()) 
vehicle_lon = float(input()) 
#toll booth location is fixed
gps_lat = 34.0522 
gps_lon = -118.2437

distance = calculate_distance(vehicle_lat, vehicle_lon, gps_lat, gps_lon)
print(f"The distance between the vehicle and the GPS location is: {distance:.2f} kilometers.")
print("Enter the time taken by the vehicle to reach the toll booth:")
time = float(input())
print("The speed code is:", calculate_speed_code(distance, time))
toll_range = 300  # Define the toll range in kilometers
if distance <= toll_range:
    # vehicle information
    print("Enter the vehicle type:")
    vehicle_type = str(input())
    print("Enter the license plate:")
    license_plate = str(input())
    data = Vehicle(vehicle_type, license_plate)

    toll_booth = TollBooth(80)  # Define the toll rate
    toll_booth.collect_toll(data)
    # Calculate the toll amount based on the vehicle type and congestion level
    congestion_level = check_congestion_level(vehicle_type)
    congestion_level = 10 + congestion_level
    print("Toll price based on the congestion level is :", congestion_level)
    if vehicle_type == "Car":
      price_per_km = congestion_level * 0.25
      toll_amount1 = price_per_km + toll_amount
    elif vehicle_type == "Truck":
      price_per_km = congestion_level * 0.50
      toll_amount1 = price_per_km + toll_amount
    elif vehicle_type == "SUV":
      price_per_km = congestion_level * 0.35
      toll_amount1 = price_per_km + toll_amount
    else:
      price_per_km = congestion_level * 0.00
      toll_amount1 = price_per_km + toll_amount
    
else:
    print("Vehicle is not within the range of toll booth.")

# Create a dummy account with an initial balance of 100

account = DummyAccount(1000)
print("Initial balance in Account is:", account.balance)
print("Total Toll amount is: ", toll_amount1)
# Deduct the toll amount
account.deduct_money(toll_amount1)
print("Deducting toll amount from the account balance...")
# Print the remaining balance
print("Remaining balance in Account is:", account.balance)
