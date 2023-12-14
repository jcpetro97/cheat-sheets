# Smartctl

**smartctl** is a command line utility designed to perform SMART tasks such as printing the SMART self-test and error logs, enabling and disabling SMART automatic testing, and initiating device self-tests.

## Usage

### Checking wear on a SSD

```bash
sudo smartctl -t short -a /dev/sdX 
sudo smartctl -a /dev/sdX
```

output:

```bash
Vendor Specific SMART Attributes with Thresholds:
ID# ATTRIBUTE_NAME          FLAG     VALUE WORST THRESH TYPE      UPDATED  WHEN_FAILED RAW_VALUE
  5 Reallocated_Sector_Ct   0x0033   100   100   010    Pre-fail  Always       -       0
  9 Power_On_Hours          0x0032   093   093   000    Old_age   Always       -       33032
 12 Power_Cycle_Count       0x0032   099   099   000    Old_age   Always       -       87
177 Wear_Leveling_Count     0x0013   097   097   000    Pre-fail  Always       -       49
```

The most important attribute to look for is “Wear_Leveling_Count”, which indicates the remaining stamina of the drive out of 100. As this number reaches to zero, your SSD is nearing failure.