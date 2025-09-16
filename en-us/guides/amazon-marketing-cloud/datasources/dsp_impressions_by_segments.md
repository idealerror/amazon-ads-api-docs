---
title: Amazon DSP impressions by segments table
description: The Amazon DSP impressions by segments table
type: guide
interface: api
---

# Amazon DSP by matched and user impressions tables

**Analytics table:** 

* `dsp_impressions_by_matched_segments`
* `dsp_impressions_by_user_segments`

**Audience table:** 

* `dsp_impressions_by_matched_segments_for_audiences`
* `dsp_impressions_by_user_segments_for_audiences`


>[WARNING] Workflows that use these tables will time out when run over extended periods of time.

These two tables have exactly the same basic structure as the [dsp_impressions](guides/amazon-marketing-cloud/datasources/dsp_impressions) table but in addition to that they provide segment level information for each of the segments that was either targeted by the ad or included the user that received the impression. This means that each impression appears multiple times in these tables. `dsp_impressions_by_matched_segments` shows only the segments that included the user and were targeted by the ad at the time of the impression. `dsp_impressions_by_user_segments` shows all segments that include the user, `behavior_segment_matched` is set to "1" if the segment was targeted by the ad.
The following table shows columns in addition to the ones listed for `dsp_impressions`.

The reference here shows only columns in addition to the ones listed for `dsp_impressions`.

## dsp\_impressions\_by\_matched\_segments

| Name                           | Data type | Metric/dimension | Description                                                                                                                                                                                                                                                                               | Aggregation threshold |
|--------------------------------|-----------|------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------|
| behavior\_segment\_description | STRING    | DIMENSION        | Description of the audience segment, for segments both targeted by the Amazon DSP line item and matched to the user at the time of impression. This field contains explanations of the characteristics that define each segment, such as shopping behaviors, demographics, and interests. | LOW                   |
| behavior\_segment\_id          | INTEGER   | DIMENSION        | Unique identifier for the audience segment, for segments both targeted by the Amazon DSP line item and matched to the user at the time of impression. Example value: '123456'.                                                                                                            | LOW                   |
| behavior\_segment\_matched     | LONG      | METRIC           | Indicator of whether the behavior segment was targeted by the Amazon DSP line item and matched to the user at the time of impression. Since this table includes matched segments only, this field will always have a value of '1' (the segment was targeted and matched to the user).     | NONE                  |
| behavior\_segment\_name        | STRING    | DIMENSION        | Name of the audience segment, for segments both targeted by the Amazon DSP line item and matched to the user at the time of impression.                                                                                                                                                   | LOW                   |



## dsp\_impressions\_by\_user\_segments

| Name                           | Data type | Metric/dimension | Description                                                                                                                                                                                                                                                                                                   | Aggregation threshold |
|--------------------------------|-----------|------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------|
| behavior\_segment\_description | STRING    | DIMENSION        | Description of the audience segment, for segments that the user belonged to at the time of impression. This field contains explanations of the characteristics that define each segment, such as shopping behaviors, demographics, and interests.                                                             | LOW                   |
| behavior\_segment\_id          | INTEGER   | DIMENSION        | Unique identifier for the audience segment, for segments that the user belonged to at the time of impression. Example value: '123456'.                                                                                                                                                                        | LOW                   |
| behavior\_segment\_matched     | LONG      | METRIC           | Indicator of whether the behavior segment was targeted by the Amazon DSP line item and matched to the user at the time of impression. Possible values for this field are: '1' (the segment was targeted and matched to the user) or '0' (the user belonged to the segment, but the segment was not targeted). | NONE                  |
| behavior\_segment\_name        | STRING    | DIMENSION        | Name of the audience segment, for segments that the user belonged to at the time of impression.                                                                                                                                                                                                               | LOW                   |
