package net.fabricmc.example;

import net.minecraft.client.Minecraft;
import net.minecraft.client.gui.screens.inventory.ContainerScreen;
import net.minecraft.network.protocol.game.ServerboundContainerClickPacket;
import net.minecraft.world.inventory.ClickType;
import net.minecraft.world.inventory.Slot;
import net.minecraft.world.item.ItemStack;
import it.unimi.dsi.fastutil.ints.Int2ObjectOpenHashMap;

public class AutoBuy {
    public static boolean enabled = false;
    public static String target = "Талисман"; // Предмет
    public static long maxPrice = 500000;      // Цена
    private static long lastAction = 0;

    public static void onTick(Minecraft mc) {
        if (!enabled || mc.screen == null) return;

        if (mc.screen instanceof ContainerScreen<?> screen) {
            String title = screen.getTitle().getString();

            if (title.contains("Аукцион") || title.contains("Поиск")) {
                for (Slot slot : screen.getMenu().slots) {
                    ItemStack is = slot.getItem();
                    if (!is.isEmpty() && is.getHoverName().getString().contains(target)) {
                        if (System.currentTimeMillis() - lastAction > 250) { // Задержка от бана
                            click(screen.getMenu().containerId, slot.index);
                            lastAction = System.currentTimeMillis();
                        }
                    }
                }
            }
            // Авто-подтверждение (зеленая шерсть)
            if (title.contains("Подтверждение")) {
                for (Slot slot : screen.getMenu().slots) {
                    if (slot.getItem().getHoverName().getString().contains("Подтвердить")) {
                        click(screen.getMenu().containerId, slot.index);
                    }
                }
            }
        }
    }

    private static void click(int id, int slot) {
        Minecraft.getInstance().getConnection().send(new ServerboundContainerClickPacket(
            id, 0, slot, 0, ClickType.PICKUP, Minecraft.getInstance().screen.getMenu().getSlot(slot).getItem(), new Int2ObjectOpenHashMap<>()
        ));
    }
}
