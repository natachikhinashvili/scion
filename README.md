# scion

### How to run
```
git clone https://github.com/natachikhinashvili/scion.git
cd scion
pip install -r requirements.txt
python3 car.py
```

## Used polymorphism:

### CarDeduplicator
```
    def detect_color(self):
        return None
```

### ToyotaDeduplicator
```
    def detect_color(self):
        wb = openpyxl.load_workbook(self.file_path)
        sheet = wb.active 

        red_columns = []
        for col in sheet.iter_cols(1, sheet.max_column):
            cell = col[0]
            if self.is_red(cell):
                red_columns.append(cell.value)

        return red_columns
```

### abstarctly it can detect any color of header, but in toyota i filter it by red header.

### CarDeduplicator
```
    def remove_duplicates(self):
        removecolumns = self.ignore_columns
        columns_to_check = [col for col in self.df.columns if col not in removecolumns]
        self.df = self.df.drop_duplicates(subset=columns_to_check)
```
### In toyota class i remove columns by header color. 
### And it can still work without using polymorphism just because i pass columns as parameters that should be ignored 

### ToyotaDeduplicator

```
    def remove_duplicates(self):
        removecolumns = self.detect_color()
        columns_to_check = [col for col in self.df.columns if col not in removecolumns]
        self.df = self.df.drop_duplicates(subset=columns_to_check)
```


### i also added is_red() function which detects if header is red but its only in toyota class
```
    def is_red(self, cell):
        fill = cell.fill
        if fill.start_color.index is not None:
            color = fill.start_color
            if color.type == "rgb" and color.rgb == "FFFF0000":
                return True
            elif color.type == "indexed" and color.indexed == 10:
                return True
        return False
```