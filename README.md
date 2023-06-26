INSERT INTO my_table (id, name, value)
SELECT updates.id, updates.name, updates.value
FROM my_table_updates AS updates
LEFT JOIN my_table ON updates.id = my_table.id
WHERE my_table.id IS NULL;

-- Step 2: Delete rows not present in the source
DELETE FROM my_table
WHERE id NOT IN (SELECT id FROM my_table_updates);

-- Step 3: Update matching rows
UPDATE my_table
SET name = updates.name, value = updates.value
FROM my_table_updates AS updates
WHERE my_table.id = updates.id;
