# Profile Picture Upload Setup Guide

## Overview
This feature allows users to upload custom profile pictures to replace their default avatars. Images are stored in Supabase Storage.

## Prerequisites
- Supabase account and project
- Project URL and anon key

## Supabase Storage Setup

### 1. Create Storage Bucket

1. Go to your Supabase Dashboard
2. Navigate to **Storage** (left sidebar)
3. Click **New bucket**
4. Name it: `profile-imgs`
5. Set as **Public bucket** ✅
6. File size limit: **500 KB**
7. Allowed MIME types: `image/jpeg`, `image/png`, `image/webp`
8. Click **Create bucket**

### 2. Set Storage Policies

The bucket needs policies to allow:
- **Public Read**: Anyone can view profile pictures
- **Authenticated Upload**: Only logged-in users can upload to their own folder

#### Policy 1: Public Read Access

```sql
CREATE POLICY "Public Access"
ON storage.objects FOR SELECT
USING ( bucket_id = 'profile-imgs' );
```

#### Policy 2: Authenticated Upload (User's Own Folder)

```sql
CREATE POLICY "Users can upload to own folder"
ON storage.objects FOR INSERT
WITH CHECK (
  bucket_id = 'profile-imgs' 
  AND auth.role() = 'authenticated'
);
```

#### Policy 3: Authenticated Delete (User's Own Files)

```sql
CREATE POLICY "Users can delete own files"
ON storage.objects FOR DELETE
USING (
  bucket_id = 'profile-imgs'
  AND auth.role() = 'authenticated'
);
```

### 3. Configure Environment Variables

#### Backend (.env in root)
```env
SUPABASE_URL=https://your-project-id.supabase.co
SUPABASE_ANON_KEY=your-anon-key-here
```

#### Frontend (client/.env)
```env
VITE_SUPABASE_URL=https://your-project-id.supabase.co
VITE_SUPABASE_ANON_KEY=your-anon-key-here
```

**Important:** Add `.env` to `.gitignore` to keep credentials secure!

### 4. Verify Setup

1. Log into your application
2. Click on your profile avatar in the top-right corner
3. Upload dialog should appear
4. Select an image (JPG, PNG, or WebP under 500 KB)
5. Click Upload
6. Check Supabase Storage → `profile-imgs` → your user folder

## File Structure

Profile pictures are stored as:
```
profile-imgs/
  └── {userId}/
      └── avatar.{ext}
```

Example:
```
profile-imgs/
  └── u1/
      └── avatar.jpg
  └── u2/
      └── avatar.png
```

## Features

✅ Click avatar to upload
✅ Image preview before upload
✅ File type validation (JPG, PNG, WebP)
✅ File size validation (max 500 KB)
✅ Auto-deletion of old avatar when uploading new one
✅ Public URLs for easy display
✅ Fallback to default avatars if not uploaded

## Troubleshooting

### Upload fails with "Supabase is not configured"
- Check that environment variables are set correctly
- Restart the development server after adding .env files

### Images don't display
- Verify bucket is set to **Public**
- Check storage policies are created
- Confirm public URL is being saved to database

### "Policy violation" errors
- Ensure storage policies are created
- Check that bucket_id matches exactly: `profile-imgs`

### File too large
- Bucket limit: 500 KB
- Compress images before uploading
- Consider using WebP format for smaller file sizes

## Optional: Avatar Library

To add a selectable avatar library (future enhancement):
1. Upload cartoon avatars to a separate `avatars/` folder in the bucket
2. Create a selection UI in the upload dialog
3. Allow users to choose between upload or library selection
