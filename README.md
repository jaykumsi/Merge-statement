MERGE INTO edata.key_audit_navisite_dml_test AS target
USING (
  SELECT pk_col1, pk_col2, dml_type, dms_date, status
  FROM edata.navisite_dml_test
  WHERE dms_date >= (SELECT last_run_date_time FROM edata.control_Tbl WHERE target_table_name = 'navisite_dml_test')
) AS source
ON (target.pk_col1 = source.pk_col1 AND target.pk_col2 = source.pk_col2)
WHEN MATCHED THEN
  UPDATE SET
    target.dml_type = source.dml_type,
    target.dms_date = source.dms_date,
    target.status = source.status
WHEN NOT MATCHED THEN
  INSERT (pk_col1, pk_col2, dml_type, dms_date, status)
  VALUES (source.pk_col1, source.pk_col2, source.dml_type, source.dms_date, source.status)
WHEN NOT MATCHED BY SOURCE THEN
  DELETE;
