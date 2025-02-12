# Uber Trip Analysis

This Jupyter notebook analyzes Uber trip data to identify patterns in trip cancellations and vehicle availability issues. The analysis focuses on temporal patterns across different time sessions and pickup points.

## 📁 Dataset
- **File**: `Uber Request Data.csv`
- **Columns**:
  - `Request id`
  - `Pickup point` (Airport/City)
  - `Status` (Trip Completed/Cancelled/No Cars Available)
  - `Request timestamp`
  - `Drop timestamp`

## 📊 Analysis Workflow

### 1. Data Preparation
- Imported libraries: `numpy`, `pandas`, `matplotlib`.
- Loaded data and converted timestamps to datetime format.
- Cleaned column names and dropped irrelevant columns (`Request_id`, `Driver_id`, `Drop_timestamp`).

### 2. Session Categorization
Trips were divided into 6 time-based sessions:
- Late Night (12 AM – 4 AM)
- Early Morning (4 AM – 8 AM)
- Late Morning (8 AM – 12 PM)
- Afternoon (12 PM – 4 PM)
- Evening (4 PM – 8 PM)
- Night (8 PM – 12 AM)

### 3. Key Visualizations
#### a. Trip Status Distribution by Session
- **Observation**: 
  - "No Cars Available" peaks in **Evening** and **Night** sessions.
  - "Cancelled" trips are most frequent in **Early Morning** and **Late Morning**.

#### b. Cancelled Trips by Pickup Point
- Analyzed cancellations across sessions and pickup locations to identify problem areas.

### 4. Some Code Snippets
**1.Session categorization**
```python
session_labels = ['Late Night', 'Early Morning', 'Late Morning', 'Afternoon', 'Evening', 'Night']
df_uber['session'] = pd.cut(df_uber.Request_timestamp.dt.hour, [-1,4,8,12,16,20,24], labels=session_labels)

# Plotting trip status distribution
plt.style.use('ggplot')
df_uber.groupby(['session','Status']).Status.count().unstack().plot.bar(figsize=(15,10))
plt.title('Total Count of Trip Statuses')
```
<img width="585" alt="Screenshot 2025-02-07 at 4 47 57 PM" src="https://github.com/user-attachments/assets/5e62f41a-2f6e-466d-8da3-23d956a95d5c" />





**2. Plotting share/frequency of all Cancelled trips over the day to identify problem areas**

``` python
plt.style.use('ggplot')
colors = ["#20B2AA", "#9400D3"]
df_tripscancelled.groupby(['session','Pickup_point']).Pickup_point.count().unstack().plot.bar(legend=True, figsize=(15,10), color=colors)
plt.title('Count and Distribution of all "Cancelled" Trips over the day')
plt.xlabel('Sessions')
plt.ylabel('Total Count of "Cancelled" Trips')
plt.show()
```
<img width="905" alt="Screenshot 2025-02-07 at 4 45 28 PM" src="https://github.com/user-attachments/assets/c2ef0440-5639-4948-ab28-c599e5eeaa89" />




**3.Plotting share/frequency of all "Trip Status" over the day to identify problem areas**
```python
plt.style.use('ggplot')
colors = ["#CC2529", "#8E8D8D","#008000"]
df_uber.groupby(['session','Status']).Status.count().unstack().plot.bar(legend=True, figsize=(15,10), color=colors)
plt.title('Total Count of all Trip Statuses')
plt.xlabel('Sessions')
plt.ylabel('Total Count of Trip Status')
plt.show()
```
<img width="907" alt="Screenshot 2025-02-07 at 4 50 21 PM" src="https://github.com/user-attachments/assets/b86bad10-061e-4de4-b3ee-37dd1de95956" />




