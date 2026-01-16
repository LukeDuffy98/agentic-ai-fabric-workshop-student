# Fabric constraints output (training sample)

This document represents constraints exported from Microsoft Fabric for training purposes.

## Restricted shift windows (avoid scheduling)

Avoid assigning on-call shifts during these windows:

- **Tuesday 12:00–16:00** (pipeline refresh window)
- **Wednesday 00:00–08:00** (lakehouse maintenance)
- **Thursday 16:00–20:00** (capacity optimization)

## Interpretation guidance

- If a schedule proposal includes any of the restricted windows above, the schedule is not valid.
- If the user does not provide dates, interpret the schedule as a week that starts on **Monday** and use day-of-week time windows.
