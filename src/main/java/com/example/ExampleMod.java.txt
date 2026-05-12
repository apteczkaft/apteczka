package net.fabricmc.example;

import net.fabricmc.api.ClientModInitializer;
import net.fabricmc.fabric.api.client.event.lifecycle.v1.ClientTickEvents;
import net.fabricmc.fabric.api.client.keybinding.v1.KeyBindingHelper;
import net.minecraft.client.KeyMapping;
import net.minecraft.network.chat.Component;
import org.lwjgl.glfw.GLFW;

public class ExampleMod implements ClientModInitializer {
    private static KeyMapping keyBinding;

    @Override
    public void onInitializeClient() {
        keyBinding = KeyBindingHelper.registerKeyBinding(new KeyMapping(
                "Включить Автобай", 
                GLFW.GLFW_KEY_R, // Кнопка R
                "FunTime Helper"
        ));

        ClientTickEvents.END_CLIENT_TICK.register(client -> {
            while (keyBinding.consumeClick()) {
                AutoBuy.enabled = !AutoBuy.enabled;
                if (client.player != null) {
                    client.player.displayClientMessage(Component.literal("§6[Bot] §fАвтобай: " + (AutoBuy.enabled ? "§aВКЛ" : "§cВЫКЛ")), true);
                }
            }
            AutoBuy.onTick(client);
        });
    }
}
