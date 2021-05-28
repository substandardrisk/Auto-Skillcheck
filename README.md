using (Bitmap img = AutoIt.CaptureScreen((int)CIRCLE_OFFSET.X, (int)CIRCLE_OFFSET.Y, (int)CIRCLE_OFFSET.X + CIRCLE_DIAMETER + 1, (int)CIRCLE_OFFSET.Y + CIRCLE_DIAMETER + 1))
{
    SkillCheckActive = false;

    int check_start_index = SkillCheckProgressIndex != -1 ? SkillCheckProgressIndex : 0;

    for (int i = check_start_index; i < 360; ++i)
    {
        Vec3 dir = Vec3.Rotate(INITIAL_DIR, i * 1, VEC_UP);

        Vec3 pixel = CIRCLE_CENTER + dir;
        Color pixel_color = img.GetPixel((int)pixel.X, (int)pixel.Y);

        if (pixel_color.R > 220 && pixel_color.G < 50 && pixel_color.B < 20)
        {
            SkillCheckActive = true;
            SkillCheckProgressIndex = i;
        }

        if ((SkillCheckPerfectZoneIndex == -1 || SkillCheckPerfectZoneIndex > i) &&
            pixel_color.R > 240 && pixel_color.G > 240 && pixel_color.B > 240)
        {
            SkillCheckPerfectZoneIndex = i;
        }

        // we found all we need, can break now
        if (SkillCheckProgressIndex != -1 && SkillCheckPerfectZoneIndex != -1)
        {
            break;
        }
    }

    if (!SkillCheckActive)
    {
        SkillCheckPerfectZoneIndex = -1;
        SkillCheckProgressIndex = -1;
    }
    
    //...
}
