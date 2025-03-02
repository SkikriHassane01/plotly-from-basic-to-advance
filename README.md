# Complete Plotly Visualization Guide
![](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQqMH8nhsOKoWW64qyoKRzdXkAgdV9rBdJG0egV8KV1gWyotk1ElpVigpe2asdO-5TrSmI&usqp=CAU)
## Setup and Data Loading
```python
# Install required packages
!pip install plotly pandas numpy

# Import necessary libraries
import plotly.express as px
import plotly.graph_objects as go
import plotly.io as pio
import pandas as pd
import numpy as np

# Load sample datasets
df_gapminder = px.data.gapminder()  # Country development data
df_iris = px.data.iris()            # Iris flower dataset
df_stocks = px.data.stocks()        # Stock market data
```

## 1. Line Plots
**Definition**: Line plots connect data points with lines, ideal for showing trends over time or continuous data sequences.

### Basic Line Plot
```python
# Single line
fig = px.line(
    df_stocks,
    x='date',
    y='GOOG',
    title='Google Stock Price'
)

# Multiple lines
fig = px.line(
    df_stocks,
    x='date',
    y=['GOOG', 'AAPL', 'AMZN'],
    title='Stock Prices Comparison'
)

# Customized line plot
fig = go.Figure()
fig.add_trace(
    go.Scatter(
        x=df_stocks['date'],
        y=df_stocks['GOOG'],
        mode='lines+markers',
        name='Google',
        line=dict(color='blue', width=2, dash='dash')
    )
)
```

## 2. Scatter Plots
**Definition**: Scatter plots display individual data points on a coordinate system, useful for showing relationships between variables.

### Basic Scatter Plot
```python
# Simple scatter
fig = px.scatter(
    df_iris,
    x='sepal_width',
    y='sepal_length',
    title='Iris Sepal Dimensions'
)

# Scatter with size and color
fig = px.scatter(
    df_gapminder[df_gapminder['year']==2007],
    x='gdpPercap',
    y='lifeExp',
    size='pop',
    color='continent',
    hover_name='country',
    size_max=60,
    title='GDP vs Life Expectancy (2007)'
)
```

## 3. Bar Charts
**Definition**: Bar charts use rectangular bars to represent data categories, perfect for comparing quantities across groups.

### Various Bar Charts
```python
# Basic bar chart
fig = px.bar(
    df_gapminder[df_gapminder['year']==2007],
    x='country',
    y='gdpPercap',
    title='GDP per Capita by Country'
)

# Grouped bar chart
fig = px.bar(
    df_gapminder,
    x='continent',
    y='pop',
    color='year',
    barmode='group',
    title='Population by Continent and Year'
)

# Stacked bar chart
fig = px.bar(
    df_gapminder,
    x='continent',
    y='pop',
    color='year',
    barmode='stack',
    title='Population by Continent and Year (Stacked)'
)
```

## 4. Histograms
**Definition**: Histograms show the distribution of numerical data by splitting it into bins and showing the frequency of values in each bin.

```python
# Basic histogram
fig = px.histogram(
    df_iris,
    x='sepal_length',
    nbins=30,
    title='Distribution of Sepal Length'
)

# Histogram with color groups
fig = px.histogram(
    df_iris,
    x='sepal_length',
    color='species',
    marginal='box',
    title='Sepal Length Distribution by Species'
)
```

## 5. Box Plots
**Definition**: Box plots show data distribution through quartiles, helping identify outliers and compare distributions across groups.

```python
# Basic box plot
fig = px.box(
    df_iris,
    x='species',
    y='sepal_length',
    title='Sepal Length Distribution by Species'
)

# Box plot with points
fig = px.box(
    df_iris,
    x='species',
    y='sepal_length',
    points='all',
    title='Sepal Length Distribution with All Points'
)
```

## 6. Violin Plots
**Definition**: Violin plots combine box plots with kernel density estimation, showing the probability density of data at different values.

```python
# Basic violin plot
fig = px.violin(
    df_iris,
    x='species',
    y='sepal_length',
    title='Sepal Length Distribution (Violin)'
)

# Violin plot with box plot inside
fig = go.Figure()
for species in df_iris['species'].unique():
    fig.add_trace(
        go.Violin(
            x=df_iris[df_iris['species']==species]['species'],
            y=df_iris[df_iris['species']==species]['sepal_length'],
            name=species,
            box_visible=True,
            meanline_visible=True
        )
    )
```

## 7. Choropleth Maps
**Definition**: Choropleth maps use color gradients to represent values across geographic regions, perfect for spatial data visualization.

### World Choropleth
```python
# Basic world choropleth
fig = px.choropleth(
    df_gapminder[df_gapminder['year']==2007],
    locations='iso_alpha',
    color='lifeExp',
    hover_name='country',
    color_continuous_scale='Viridis',
    title='Life Expectancy by Country (2007)'
)

# US States choropleth
df_states = px.data.election()  # Sample US states data
fig = px.choropleth(
    df_states,
    locations='state',
    locationmode='USA-states',
    color='Votes',
    scope='usa',
    title='US States Example'
)
```

## 8. Animated Plots
**Definition**: Animated plots show how data changes over time or across categories by creating smooth transitions between frames.

### Animated Scatter
```python
# Animated bubble chart
fig = px.scatter(
    df_gapminder,
    x='gdpPercap',
    y='lifeExp',
    size='pop',
    color='continent',
    hover_name='country',
    animation_frame='year',
    animation_group='country',
    size_max=55,
    title='GDP vs Life Expectancy Over Time'
)

# Animation settings
fig.update_layout(
    updatemenus=[{
        'type': 'buttons',
        'showactive': False,
        'buttons': [{
            'label': 'Play',
            'method': 'animate',
            'args': [None, {
                'frame': {'duration': 500, 'redraw': True},
                'fromcurrent': True
            }]
        }, {
            'label': 'Pause',
            'method': 'animate',
            'args': [[None], {
                'frame': {'duration': 0, 'redraw': False},
                'mode': 'immediate'
            }]
        }]
    }]
)
```

### Animated Choropleth
```python
# Animated choropleth map
fig = px.choropleth(
    df_gapminder,
    locations='iso_alpha',
    color='lifeExp',
    hover_name='country',
    animation_frame='year',
    title='Life Expectancy Evolution',
    color_continuous_scale='Viridis'
)
```

## 9. 3D Plots
**Definition**: 3D plots visualize data in three dimensions, useful for showing relationships between three variables.

### 3D Scatter
```python
# 3D scatter plot
fig = px.scatter_3d(
    df_iris,
    x='sepal_length',
    y='sepal_width',
    z='petal_length',
    color='species',
    title='Iris Measurements in 3D'
)

# 3D line plot
fig = go.Figure(data=[
    go.Scatter3d(
        x=np.random.randn(100),
        y=np.random.randn(100),
        z=np.random.randn(100),
        mode='lines+markers'
    )
])
```

## 10. Heatmaps
**Definition**: Heatmaps use color intensity to represent values in a matrix format, useful for showing patterns in 2D data.

```python
# Basic heatmap
correlation_matrix = df_iris.corr()
fig = px.imshow(
    correlation_matrix,
    title='Correlation Heatmap'
)

# Advanced heatmap
fig = go.Figure(data=go.Heatmap(
    z=correlation_matrix,
    x=correlation_matrix.columns,
    y=correlation_matrix.columns,
    colorscale='RdBu',
    zmin=-1,
    zmax=1
))
```

## 11. Pie and Donut Charts
**Definition**: Pie charts show parts of a whole by dividing a circle into proportional segments.

```python
# Pie chart
fig = px.pie(
    df_gapminder[df_gapminder['year']==2007],
    values='pop',
    names='continent',
    title='Population by Continent'
)

# Donut chart
fig = px.pie(
    df_gapminder[df_gapminder['year']==2007],
    values='pop',
    names='continent',
    title='Population by Continent',
    hole=0.4  # Makes it a donut chart
)
```

## 12. Advanced Customization

### Layout Customization
```python
fig.update_layout(
    title={
        'text': "Main Title",
        'y':0.95,
        'x':0.5,
        'xanchor': 'center',
        'yanchor': 'top'
    },
    template='plotly_dark',
    margin=dict(l=40, r=40, t=40, b=40),
    showlegend=True,
    legend=dict(
        yanchor="top",
        y=0.99,
        xanchor="left",
        x=0.01
    )
)
```

### Axes Customization
```python
fig.update_xaxes(
    title_text="X Axis Title",
    type='log',  # 'linear', 'log', 'date', 'category'
    showgrid=True,
    gridwidth=1,
    gridcolor='LightGray'
)

fig.update_yaxes(
    title_text="Y Axis Title",
    type='linear',
    showticklabels=True,
    tickformat=',.0f'
)
```

### Interactive Features
```python
# Custom hover template
fig.update_traces(
    hovertemplate="""
    <b>%{x}</b><br>
    Value: %{y:,.2f}<br>
    <extra></extra>
    """
)

# Add annotations
fig.add_annotation(
    x=2,
    y=4,
    text="Important Point",
    showarrow=True,
    arrowhead=1
)
```

## 13. Saving and Exporting
```python
# Save as interactive HTML
fig.write_html("plot.html")

# Save as static images
fig.write_image("plot.png")
fig.write_image("plot.pdf")
fig.write_image("plot.svg")
```

This guide covers all essential Plotly visualizations with definitions and practical examples. Each section includes both basic and advanced usage, making it a comprehensive reference for data visualization.
