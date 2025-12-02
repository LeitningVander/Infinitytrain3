# Supabase Database Migration

## Add resource_links column to subtopics table

If you're using Supabase (production), you need to run this SQL command in the Supabase SQL Editor:

```sql
ALTER TABLE subtopics 
ADD COLUMN resource_links TEXT;
```

This adds a new column to store video and document links as JSON for each subtopic.

## For SQLite (local development)

The SQLite database will automatically create the new column when you restart the server, or you can delete the `training.db` file to recreate it with the new schema.
