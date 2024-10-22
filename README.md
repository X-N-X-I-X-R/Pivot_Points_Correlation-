
# Volume and Pivot Points Correlation with Full Control

This Pine Script indicator allows for detailed control over the display of pivot points (high and low) and volume correlations on a price chart. It provides customizable visual cues like lines and labels for unusual volume levels in correlation with pivot points. The user has full control over what elements to display, including colors, labels, and pivot point ranges.

## Features
- Display pivot points for high and low values.
- Show support and resistance lines based on pivot points.
- Customize labels for pivot points and price levels (e.g., "War Zone," "Wait," "Overbought," and "Oversold").
- Customize text and line colors for both high and low pivots.
- Control visibility of pivots, lines, and labels individually for high and low pivots.
- Detect unusual volume levels compared to a user-defined volume moving average and standard deviation factor.

## How It Works

The script identifies pivot high and pivot low points over a user-specified number of bars to the left and right of the pivot point. It then checks for unusual volume, defined as volume that is significantly above or below a moving average, adjusted by a factor of standard deviation. Based on this, the script displays lines and labels to indicate possible "zones" of market behavior.

## Input Parameters

### General Controls
- **Show High Pivots:** Enables or disables the display of high pivot points.
- **Show High Lines:** Enables or disables support and resistance lines for high pivots.
- **Show High Labels:** Enables or disables labels for high pivots.
- **Show Low Pivots:** Enables or disables the display of low pivot points.
- **Show Low Lines:** Enables or disables support and resistance lines for low pivots.
- **Show Low Labels:** Enables or disables labels for low pivots.

### Pivot Point Settings
- **Pivot High (Left and Right):** Sets the number of bars to the left and right of the pivot high to define the pivot point.
- **Pivot Low (Left and Right):** Sets the number of bars to the left and right of the pivot low to define the pivot point.
- **Pivot High Label Color:** Sets the label color for high pivots.
- **Pivot High Line Color:** Sets the line color for high pivots.
- **Pivot Low Label Color:** Sets the label color for low pivots.
- **Pivot Low Line Color:** Sets the line color for low pivots.

### Volume Settings
- **Volume Moving Average Length:** Sets the length of the moving average for volume calculations.
- **Volume Threshold Factor:** Multiplies the standard deviation to define unusual volume levels.

### Label Text (Examples)
- **High Pivots:**
  - "War Zone" for areas around the pivot.
  - "Wait" for indecision areas.
  - "Overbought" for extreme high price levels.
- **Low Pivots:**
  - "War Zone" for areas around the pivot.
  - "Wait" for indecision areas.
  - "Oversold" for extreme low price levels.

## Code Breakdown

### Pivot Calculations

- **Pivot High (`ph`):** `ta.pivothigh(leftP, rightP)` finds the high pivot points using the left and right bar counts.
- **Pivot Low (`pl`):** `ta.pivotlow(lpivot, rpivot)` finds the low pivot points using the left and right bar counts.

### Volume Calculations

The script calculates the moving average of volume and its standard deviation. These are used to define "unusual volume":
```pinescript
volume_ma = ta.sma(volume, volume_ma_length)
volume_std_dev = ta.stdev(volume, volume_ma_length)
is_unusual_volume_high = volume > (volume_ma + volume_std_dev * volume_threshold_factor)
is_unusual_volume_low = volume < (volume_ma - volume_std_dev * volume_threshold_factor)
```

### Line and Label Drawing

- **Lines:** Lines are drawn for support and resistance based on the calculated pivot points and the presence of unusual volume. Different styles and colors can be configured.
  ```pinescript
  line.new(x1=bar_index[rightLenH]+ 60, y1=pivot_10_up, x2=bar_index, y2=pivot_10_up, color=lineColorH, width=2, style=line.style_dotted)
  ```

- **Labels:** Labels are drawn above or below the pivot points to indicate significant levels such as "War Zone," "Wait," or "Overbought."
  ```pinescript
  label.new(bar_index- 5, pivot_5_up, text="War Zone", style=label.style_label_center, color=backgcolor, textcolor=labelColorH, size=size.small)
  ```

### Function to Draw Labels

This function allows drawing custom labels at the pivot points:
```pinescript
drawLabel(_offset, _pivot, _style, _color, _textColor) =>
    if not na(_pivot)
        label.new(bar_index[_offset], _pivot, str.tostring(_pivot, format.mintick), style=_style, color=_color, textcolor=_textColor)
```

## Usage

1. **Customization:** Use the input parameters to customize the number of bars used for pivot detection, the colors of the lines and labels, and whether to display high or low pivots.
2. **Unusual Volume Detection:** Adjust the `Volume Threshold Factor` to control the sensitivity of unusual volume detection.
3. **Display Control:** Toggle the visibility of high and low pivots, lines, and labels to focus on the most relevant data for your analysis.

## Example

If you set the following values:
- **Pivot High (Left and Right):** 5
- **Volume Moving Average Length:** 10
- **Volume Threshold Factor:** 1.5

The script will draw pivot points that span 5 bars left and right, calculate a 10-bar moving average for volume, and flag volumes that exceed 1.5 standard deviations as "unusual."
![Example Screenshot](./Screenshot%202024-10-22%20at%2012.58.15.png)

