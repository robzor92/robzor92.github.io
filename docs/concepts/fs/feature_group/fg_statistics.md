HSFS supports monitoring, validation, and alerting for features:

 - transparently compute statistics over features on writing to a feature group;
 - validation of data written to feature groups using Great Expectations
 - alerting users when there was a problem writing or update features. 


You can define expectation suites in Great Expectations and assoicate them with feature groups. When you write to a feature group, the expectations are executed, then you can define a policy on the feature group for what to do if any expectation fails.

<img src="/assets/images/concepts/fs/fg-expectations.svg">

### Alerting

HSFS also supports alerts that can be triggered when expectations fail. For example, you can send a slack message if features being written to a feature group are missing some input data.
