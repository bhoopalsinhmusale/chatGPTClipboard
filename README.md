# chatGPTClipboard

DECLARE @n INT = 3;  -- Replace with the actual number of tables

DECLARE @counter INT = 1;
DECLARE @sql NVARCHAR(MAX) = N'SELECT uuid, column1, column2, ... FROM ';

IF @n > 0
BEGIN
    WHILE @counter <= @n
    BEGIN
        DECLARE @table_name NVARCHAR(100);
        SET @table_name = 'table_name_' + CAST(@counter AS NVARCHAR(10));

        SET @sql = @sql + QUOTENAME(@table_name);
        
        IF @counter < @n
        BEGIN
            SET @sql = @sql + N' JOIN ';
        END
        
        SET @counter = @counter + 1;
    END;

    IF @n > 1
    BEGIN
        SET @sql = @sql + N' ON ' + @table_name + '.uuid = ' + @table_name + '_1.uuid';
    END;
END;

EXEC sp_executesql @sql;
