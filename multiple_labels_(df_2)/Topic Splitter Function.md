# Topic Splitter Function

This Python function splits topics and adjusts associated values based on the number of sub-topics.

## Usage

```python
import pandas as pd

def split_topics_and_adjust_value(row):
    """
    Process topic labels for each row and adjust associated value.

    Args:
    row (pd.Series): A row from the DataFrame with 'Main Topic', 'Sub Topic', and 'Value' columns

    Returns:
    pd.DataFrame: New rows after splitting topics and adjusting value
    """
    main_topics = row['Main Topic'].split(',')
    sub_topics = row['Sub Topic'].split(',')
    
    try:
        value = float(row['Value'])
    except ValueError:
        value = 0
    
    new_rows = []
    for main_topic in main_topics:
        related_sub_topics = [
            sub for sub in sub_topics
            if not topic_reference[
                (topic_reference['Main Topic'] == main_topic) & 
                (topic_reference['Sub Topic'] == sub)
            ].empty
        ]
        
        num_sub_topics = len(related_sub_topics)
        
        if num_sub_topics == 0:
            continue
        
        for sub_topic in related_sub_topics:
            new_row = row.copy()
            new_row['Main Topic'] = main_topic
            new_row['Sub Topic'] = sub_topic
            new_row['Value'] = value / num_sub_topics
            new_rows.append(new_row)
    
    return pd.DataFrame(new_rows)

# Example usage
if __name__ == "__main__":
    # Create sample DataFrames and apply the function
    # (Add your example code here)
    pass
```

## Requirements
- Python 3.x
- pandas

## Note
This function requires a `topic_reference` DataFrame to be defined, which should contain the relationships between main topics and sub-topics.
